# usports-fb-xml-check
USports Football XML Check

This Colab validates a Football XML file from either Presto or Stats Crew. It can be run at halftime, or the end of the game.

## Usage

1. You should be logged in with a Google specific account (ie gmail)
1. View the XML_Play_Validator.ipynb in Github and it will have a link to Open in Colab in the upper left
1. Click Connect in the upper right
1. Click Run All in the top bar
     - You may be prompted with a warning that the Colab will have access to your Google resources. Click Run Anways. The Colab only accesses the XML file.
1. You will be promoted to upload a .txt file for visitor and home rosters. You can use the printable versions from each team's website and copy it into a text file. You should be able to just ctrl+A (select all) and then ctrl+v (paste) the content into the text file.
      - This should work for Sidearm schools, but may have issues for other prinatable rosters.
1. You will be prompted in the first cell to upload the XML. Click Choose File and select the XML file in question and click upload.
1. Wait for the process to complete and scroll to the bottom
1. In the last cell there will be a report that you can review with the issues detected

## What it does:

It does the below checks for the input file and provides the play # and play where it detects an issue. It separates the issues into either errors or warnings. Errors are typically problems that need to be fixed. Warnings are potential issues, but could also be fine depending on the play.

**Check 1: Clock & Possession Change Checks**
- Verifies that the clock time is present and differs from the previous play on major scoring plays (excluding touchdowns), change of possessions, and kick sequences.
- Where the clock appears unchanged across adjacent plays, checks the record marking the start of the next drive; if it shows a different time, this confirms the clock did advance and downgrades the issue to a warning instead of an error.
- Enforces clock bounds checks, ensuring clock values do not exceed the 15-minute regulation limit or drop below zero.

**Check 2: Penalty Code & Yardage Validation**
- Audits penalty codes against the full official Canadian penalty code sheet (~50 codes) and their standard yardage distances.
- Flags American penalty terminology (such as PF or UC) in favor of Canadian terms.
- Flags any accepted penalty assessed exactly 0 yards for review, unless 0 yards is the documented standard for that specific code (e.g. disqualification-only penalties).
- Skips distance checks if a penalty is explicitly declined.
- Notes plays starting inside the 15-yard line to account for potential distance caps.

**Check 3: Player Position Anomaly Validation**
*Requires the visitor and home rosters to be uploaded at the start of the Colab (see Usage, step 5).*

- Flags plays where a pass is thrown by a player not listed as a QB (or a QB in a multi-position listing such as QB/WR).
- Flags plays where a player listed at a defensive position (DB, CB, LB, DL, S) is credited with an offensive action such as a rush or reception.
- Flags plays where the player credited with a kickoff, punt, or field goal isn't listed as a kicker/punter (K, P, or a multi-position listing such as K/WR), based on who actually performed the kick rather than every name mentioned in the play (e.g. returners or tacklers).

**Check 4: Passing vs. Receiving Yards Validation**
- Aggregates and compares total team quarterback passing yards against cumulative receiver yards for each team.

**Check 5: Large Yardage Loss Detection**
- Scans individual plays for major losses, sacks, or negative yardage metrics of 20 yards or greater, checking both the play description and the underlying stat attributes (including plays recorded as a net gain/loss rather than an explicit "loss" value).

**Check 6: Unauthorized TEAM / TM Stat Attribution & Team Rush Losses**
- Flags plays where generic entities (TEAM or TM) are incorrectly credited with statistics or text attributions (excluding valid team safeties and penalties).
- Prevents duplicate reporting on single plays by using a unified tracking gate.

**Time of Possession Validation**
- Sums each team's reported time of possession and compares the combined total against the expected game length (30 minutes for a half, 60 minutes for a full game), flagging a mismatch as an error.

**Player Stat Entry Logic Validation**
- Scans individual player stat records to ensure logical consistency, such as checking that completions do not exceed passing attempts and that values are non-negative.
