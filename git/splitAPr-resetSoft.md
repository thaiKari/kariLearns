## Scenario:
you have started developing a new feature from another feature branch and want to have a clean PR into master with only the relevant changes. Works if the new featrue code is seperated in other files than the unrelated code.

### Steps:
1. Checkout a new branch for the feature.
2. Perform a soft reset: `git reset --soft master`
3. Check git status to see which files are affected: `git status`
4. restore those files that you dont want included in your PR: `git restore --staged UnrelatedFile.cs`
5. `git commit -m'remove unrelated code'`
6. `git push origin featureBranch --force`