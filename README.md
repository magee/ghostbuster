# ghostbuster

This tool helps identify students who might be ghosting, struggling or otherwise flying under the radar during the sprint phase of the program as well as the group phase of the program. This tool is intended to alert support staff to investigate more deeply and is not intended as a judgment or diagnostic.

# Prerequisites
You'll need to set up four things:

* A config folder at the root of the project with a config.js file inside. In this config file, paste the auth token for the github API in the following format:
```
module.exports = {
  AUTH_GITHUB_TOKEN:  'your_token_here'
}
```
* A teams.js file in the config folder
three variables, exported as module.exports: thesisTeams, greenfieldTeams, legacyTeams
Each of these variables takes the following format:
```
thesisTeams: {
  team_name_here: {
    github: "github_org_name_here",
    students: [
      {
        firstName: "student_first_name_here",
        github: "student_github_handle_here"
      }
    ]
  }
}
```
* A cohorts.js file in the config folder.  Note that while the entire variable is upper case (RPT11) to match the attendance tool, the name property needs to be lower case in order to properly match the names of the repos on github.
```
const RPT11 = {
  name: 'rpt11',
  students: [
    {
      firstName: 'Student first name here',
      lastName: 'Student last name here',
      github: 'Student github here'
    }]
}
module.exports = { RPT11 }
```
* A sprints.js file in the config folder.  This file holds all the milestone commit messages for each sprint
```
const allSprints = {
  "sprint-name-here (leave off cohort prefix)": [
    {message:"milestone commit message (lower case)"},
    {message:"complete part 1"},
    {message:"complete through defaults"},
    {message:"complete bare minimum requirements"}
  ],
}
module.exports = { allSprints }
```
Each time a cohort enters a new part of the project phase, you can update their info in the appropriate place in teams.js and the ghostbuster will automatically check them all

Each time a new cohort begins the RPT program, you can add their info to cohorts.js.  Each time a cohort graduates, their info can be optionally removed from this file.  Be sure to import the new cohorts into checkSprints.js.

When sprints change, or are added or removed, simply adjust the commit milesone messages in sprints.js.
# Installing
run npm install

Running the ghostbuster for projects- to see data by team for the previous week
run this command from the root of the project ```node ghostbuster```

Running the checker - to see lifetime contributions by student
run this command from the root of the project ```node contributions```

Running the sprint checker - to see student progress on sprints. For each cohort you want to check, at the bottom of checkSprints.js call printForCohort with that cohort name, and an array with a list of all sprints you'd like to check.
``` printForCohort(COHORT, ['name-of-sprint-here', 'name-of-second-sprint-here]);```
run this command from the root of the project ```node checkSprints```

# Limitations
This tool currently only checks the default branch (master) of each repo for project phase.

This tool currently relies on legacy projects working off their own copy of the original repo.  If a legacy team works directly with source code of the original greenfield, the stats will be inaccurate.

This tool currently relies on students properly using the milestone commits for sprints for accurate stats on their progress.

# Customizing
* Right now the tool checks for the previous week's commits in projects. You can change the duration by adjusting the daysAgo variable in ghostBuster.js. A future iteration could have a customizable range by inputing a start date and end date to the github query
* I'm not sure if it's worth the effort to have this tool dynamically pull student data from google sheets, but if at some point it feels worth it, that could be a next step
* This tool does not analyze code quality for project phase - merely counts the number of commits and number of code changes.
* This tool does not analyze code quality for sprints - merely checks progress by milestone commit messages.
