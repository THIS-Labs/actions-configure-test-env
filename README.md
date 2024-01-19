# actions-configure-test-env
Composite GitHub action to configure testing environment in CI/CD pipelines

## Configuration of testing account
The AWS account used for testing depends on who triggered a workflow
run (in other words, the GitHub actor).

Up until version 0.16 of this composite action, the mapping of
actors to accounts was hardcoded in this composite action.
This meant that whenever changes were made to the team (whenever
a new developer joined, for example), a new version of this action
had to be released with the updated mapping. And all workflow files
in all stacks using this composite action had to also be updated to use
the new version.

From version 0.17.1 onward, this composite action requires one more input,
actor2account_map, which is a JSON string representing a mapping of GitHub
users (actors) to AWS accounts (for example: 
{"afs25": "amp205", "sophiemachin": "sem86", 
"this-andy": "amp205", "asim09": "afs25"}).

The calling workflow must therefore pass on the mapping when using this
composite action and it should use the default value, which is stored 
as organization variable ACTOR2ACCOUNT_MAP in our GitHub Organisation.

Typical usage of version >0.17.1 therefore looks like this:

```
  - name: Configure testing environment
    uses: THIS-Labs/actions-configure-test-env@v0.17.1
    with:
      afs25_account: ${{ secrets.AFS25_AWS_ACCOUNT }}
      sem86_account: ${{ secrets.SEM86_AWS_ACCOUNT }}
      amp205_account: ${{ secrets.AMP205_AWS_ACCOUNT }}
      actor2account_map: ${{ vars.ACTOR2ACCOUNT_MAP }}
```