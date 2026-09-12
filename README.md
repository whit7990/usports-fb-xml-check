# usports-fb-xml-check
USports Football XML Check

This Colab validates a Football XML file from either Presto or Stats Crew. It can be run at halftime, or the end of the game.

## Usage

- You should be logged in with a Google specific account (ie gmail)
- View the XML_Play_Validator.ipynb in Github and it will have a link to Open in Colab in the upper left
- Click Connect in the upper right
- Click Run All in the top bar
- You will be prompted in the first cell to upload the XML. Select the XMl file in question and click upload
- Wait for the process to complete and scroll to the bottom
- In the last cell there will be a report that you can review with the issues detected

## What it does:

It does the below checks for the input file and provides the play # and play where it detects an issue. It separates the issues into either errors or warnings. Errors are typically problems that need to be fixed. Warnings are potential issues, but could also be fine depending on the play.

**Check 1: Clock & Possession Change Checks**
- Verifies that the clock time is present and differs from the previous play on major scoring plays (excluding touchdowns), change of possessions, and kick sequences.
- Enforces clock bounds checks, ensuring clock values do not exceed the 15-minute regulation limit or drop below zero.

**Check 2: Penalty Code & Yardage Validation**
- Audits Canadian penalty codes and ensures standard distance assignments.
- Flags American penalty terminology (such as PF or UC) in favor of Canadian terms.
- Skips distance checks if a penalty is explicitly declined.
- Notes plays starting inside the 15-yard line to account for potential distance caps.

**Check 4: Passing vs. Receiving Yards Validation**
- Aggregates and compares total team quarterback passing yards against cumulative receiver yards for each team.

**Check 5: Large Yardage Loss Detection**
- Scans individual plays for major losses, sacks, or negative yardage metrics of 20 yards or greater.

**Check 6: Unauthorized TEAM / TM Stat Attribution & Team Rush Losses**
- Flags plays where generic entities (TEAM or TM) are incorrectly credited with statistics or text attributions (excluding valid team safeties and penalties).
- Prevents duplicate reporting on single plays by using a unified tracking gate.

**Player Stat Entry Logic Validation**
- Scans individual player stat records to ensure logical consistency, such as checking that completions do not exceed passing attempts and that values are non-negative.
