saas-ui status — <project name>
Last updated: <YYYY-MM-DD>

Bootstrap     <progress bar> <state text>
              <optional: current phase>
              <optional: resume command>

Posture       <NAME> — committed <N> days ago <stale flag if applicable>
              Re-evaluate at: <trigger text>

Inventory     <N> atoms · <N> molecules · <N> gaps
              <N> P0 gaps blocking composites:
              - <atom>: blocks <composite-list>

Composites    Name              Status                Blocks N modules
              ----------------- --------------------- ----------------
              <name>            <pill>                <N> modules
              <name>            <pill>                <N> modules
              ...

Modules       Name (tier)       Status              Blocking composites
              ----------------- ------------------- -------------------
              <name> (<tier>)   <pill>              <list or (none)>
              ...

Warnings      ⚠ <warning line>
              ⚠ <warning line>

Next          Recommended actions, posture-tuned for <POSTURE>:
              1. <command>
                 — <reason>
              2. <command>
                 — <reason>
              3. <command>
                 — <reason>

Auto-heal available:
              - <item>: STATE.md → <correction>
              Run /saas-ui-status --heal to apply.
