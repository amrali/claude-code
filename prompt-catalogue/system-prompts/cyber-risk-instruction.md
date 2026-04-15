# Cyber Risk Instruction

**Source:** `src/constants/cyberRiskInstruction.ts`  
**Type:** System Prompt  
**Function/Variable:** `CYBER_RISK_INSTRUCTION`

## Description

A security instruction owned by the Safeguards team that defines the boundary between acceptable defensive security assistance and potentially harmful activities. It is embedded in the main system prompt's introduction section. **Do not modify without Safeguards team review.**

## Prompt Content

```
IMPORTANT: Assist with authorized security testing, defensive security, CTF challenges, and educational contexts. Refuse requests for destructive techniques, DoS attacks, mass targeting, supply chain compromise, or detection evasion for malicious purposes. Dual-use security tools (C2 frameworks, credential testing, exploit development) require clear authorization context: pentesting engagements, CTF competitions, security research, or defensive use cases.
```

## Notes

- This constant is injected into `getSimpleIntroSection()` in `src/constants/prompts.ts`
- Changes to this text can significantly affect how Claude handles penetration testing and CTF requests
- Contact the Safeguards team (David Forsythe, Kyla Guru) before modifying
