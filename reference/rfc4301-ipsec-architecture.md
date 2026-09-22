# RFC 4301 — Security Architecture for IP: the system, not the primitives

*Original summary and analysis by Bruce (InfoSec Lead), written at John's request after finding the actual RFC dense reading. This is not a reproduction of the RFC text — it's independent commentary on how the architecture fits together.*

You already know AH/ESP/IKE/SPI/mode mechanics — the value of 4301 is that it's not really about crypto, it's a *policy enforcement architecture* bolted onto the IP layer. Three databases, a boundary, two processing paths.

## 1. The three-database model

- **SPD** — the ACL. Ordered, first-match semantics like a firewall. Formalized as SPD-S (PROTECT traffic) plus SPD-O/SPD-I (outbound/inbound BYPASS or DISCARD — asymmetric by design). Every entry resolves to PROTECT / BYPASS / DISCARD. SPD is the policy authority both SAD and PAD answer to.
- **SAD** — the live state table. One entry per unidirectional SA: SPI, algorithms/keys, sequence counter + anti-replay window, lifetimes, mode/tunnel endpoints, and the selector values actually bound (which may be narrower than the SPD allowed — see PFP below). SAD is what packet processing hits at line rate.
- **PAD** — the genuinely new piece vs. RFC 2401, easy to skim past. Sits between IKE and SPD, answering what a successfully-authenticated peer is actually authorized to claim SAs for. Without it, a valid IKE auth could negotiate any SA it likes; PAD is what stops authentication success from becoming authorization bypass.

Flow: **IKE negotiates → PAD authorizes the peer against allowed SPD ranges → SPD entry matched → SAD entry created → packet processing runs off SAD, SPD stays the source of truth.**

## 2. Selectors and PFP

Match criteria include address ranges, next-layer protocol, ports, ICMP type+code as one 16-bit selector, IPv6 mobility header type. ANY (wildcard) and OPAQUE (value exists but hidden from the selector engine — non-initial fragments, nested IPsec) are the special cases. The **PFP flag** on an SPD entry decides SA granularity: PFP=true narrows the SAD entry to the triggering packet's actual values (per-flow SA); PFP=false inherits the SPD's wildcarded range (coarse SA). This is the formal answer to why some stacks do one SA per flow and others one SA per subnet pair.

## 3. Outbound processing (Section 5.1)

Ordered SPD lookup → DISCARD / BYPASS / PROTECT (find or trigger a SAD entry via PAD/IKE, apply AH/ESP per that entry). Tunnel mode adds outer-header construction bound to the SAD entry's endpoints, independent of the inner packet.

## 4. Inbound processing (Section 5.2) — the security-critical bit

SPI pulled from the AH/ESP header, SAD lookup is longest-match: (SPI, dest, source) → (SPI, dest) → SPI alone. Crypto/anti-replay verified against that entry. Then — critically — the **decapsulated inner packet's selectors get re-checked against the SPD**, not just trusted because the SA authenticated. That re-check is what stops a valid SA from being used to smuggle traffic outside its negotiated class. Arguably the single most important integrity property in the whole model.

## 5. AH/ESP positioning

Not redefined, just positioned as the enforcement mechanism for a PROTECT verdict: SPD says PROTECT, SAD says protocol/mode/algorithm, AH/ESP carries it out. ESP with confidentiality but no integrity: NOT RECOMMENDED. NULL encryption + no integrity simultaneously: disallowed, must be an auditable event.

## 6. Mode selection (Section 4.1)

If either endpoint is a security gateway, SA MUST be tunnel mode, except traffic destined to the gateway itself (management) or gateway-to-gateway/host cases where both ends are the actual traffic endpoints. This is what formally rules out "transport mode between two gateways for transit traffic."

## 7. Key management interface

Assumes IKEv2 or equivalent, but SPD/SAD/PAD are protocol-agnostic (manual keying still normatively supported). IKE must be able to negotiate multiple SAs with identical selectors distinguished only by DSCP.

## 8. Anti-replay gotcha

DSCP-differentiated traffic multiplexed onto one SA/SPI can see legitimate low-priority packets pushed outside the replay window by differential delay and dropped as false replays. That's the real justification for per-DSCP-class SAs.

## 9. Fragmentation (Section 7/8.2)

Non-initial fragments lack the fields needed for selector matching. 4301 requires stateful reassembly before SPD matching, explicit BYPASS/DISCARD handling for fragments, or tunnel-mode-only. Flat rule: AH/ESP transport mode cannot apply to IPv4 fragments.

## 10. SPD ordering

Explicitly ordered, first-match on overlap, same as firewall rules. Appendix B's decorrelation algorithm is an optional internal optimization — 4301 specifies external observable behavior, not internal structure.

## Bottom line

PAD constrains what an authenticated identity may claim; SPD is the ordered policy authority that both creates SAD entries and re-validates decapsulated inbound traffic; SAD is the fast-path AH/ESP actually runs against. The property 4301 is precise about that its predecessor wasn't: SA authentication is necessary but not sufficient — the inbound SPD re-check is what closes the gap.

---

*Source: RFC 4301, "Security Architecture for the Internet Protocol", https://www.rfc-editor.org/rfc/rfc4301*
