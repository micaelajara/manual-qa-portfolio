# Bug report template

The format I use for every bug. The two fields most templates skip, **Environment** and **Ruled out**, are the ones that have saved the most time: once I lost close to an hour chasing a bug reported from a screenshot that didn't say which environment it came from, and two code hypotheses were tested and dropped without anyone writing down why.

```
Title: verb + what fails + where
       e.g. "Checkout summary overflows when the address has 100+ characters"

Environment:  Local (branch + commit) | Preview (URL + PR #) | Production (URL + date and time)
Test data:    the exact product / user / record that triggers it
Severity:     Critical | High | Medium | Low   (by business impact, not by how hard it was to find)
Related case: test case ID that found it, if any

Steps to reproduce:
1.
2.
3.

Actual result:
Expected result:

Ruled out (if I investigated before reporting):
- Hypothesis tested → why it doesn't explain the bug

Evidence: screenshot or video, API response, console or network log
Retest notes: date + result, every time it's retested
```

## Rules I follow

- **One bug per report.** If two things fail, they're two reports that link to each other.
- **Steps someone else can follow cold.** If a developer has to ask me how to reproduce it, the report isn't done.
- **Actual vs. expected, never "it doesn't work."**
- **Evidence from the layer where it fails.** A UI bug gets a screenshot; a data bug gets the API response that proves it.
- **Retest before closing**, and write down the result even when it's "still broken."
