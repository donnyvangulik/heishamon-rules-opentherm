# HeishaMon Rules: DHW Sterilization Improvements

This repository contains rules tailored for **HeishaMon**-driven Panasonic Aquarea systems. The focus is ensuring **DHW sterilization reliably exceeds 50 °C** by:

- Disabling **Quiet Mode** during active sterilization (`@Sterilization_State == 1`), so the compressor can ramp.
- Allowing higher **pump-duty headroom** (clamped to 140) on the **DHW path** to improve heat transfer, while keeping the dynamic safeguards for **space heating (CH)**.

## Files

- `Heishamon_rules20241129b.txt` – full ruleset (drop-in).

## How it works

- `on QuietMode`: sets Quiet Mode to `0` while sterilizing; otherwise follows your regular dynamic logic.
- `on pumpDuty`: clamps `SetMaxPumpDuty` to **120–140**. DHW/sterilization get the higher ceiling; CH keeps the dynamic cap and ratio-based protection.

These rules interact with HeishaMon over MQTT topics (e.g., `SetQuietMode`, `Quiet_Mode_Level`, `SetMaxPumpDuty`, `Operating_Mode_State`, etc.). Adjust topic names in your Heishamon device setup if you use non-default naming.

## Install

1. Open your Heishamon rules page (or wherever you run your rules against HeishaMon topics).
2. Paste the contents of `Heishamon_rules20241129b.txt`.
3. Save & reboot the node, or reload rules.
4. Trigger a sterilization cycle (or wait for the scheduler) and observe:
   - Quiet Mode goes to `0` while sterilizing.
   - `SetMaxPumpDuty` rises to `140` on DHW and returns to dynamic limits afterward.
   - DHW temperature climbs beyond **50 °C** to complete sterilization.

## Credits & references

- **HeishaMon** project: <https://github.com/Egyras/HeishaMon>  
- Inspiration & discussion: **gathering.tweakers.net** community threads on Aquarea + HeishaMon.  
- Fundamental programming approach: **HeishaMon Rules with OpenTherm Thermostat** by @blb4github  
  <https://github.com/blb4github/HeishaMon-Rules-with-Opentherm-Thermostat>

## Safety notes

- The pump-duty clamp remains **120–140**, and the flow ratio safeguard stays active to avoid low-flow/high-head operation.
- If you maintain very low DHW setpoints, some firmwares may be conservative; sterilization logic in these rules explicitly forces the needed conditions.

## License

If you fork or reuse, please keep the above credits and links intact.
