# actions-configure-test-env
Composite GitHub action to configure testing environment in CI/CD pipelines

## Configuration of testing account

### Versions 0.x
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

### Versions 1.x
Versions 0.x of this composite action expected the calling action to provide many inputs, 
with variants for each GitHub actor (for example, `afs25_sg_account` and `sem86_sg_account`)
for the security groups in those accounts.

From version 1.0, parameters are stored in a JSON variable actor2env_map, which is
passed in as a single input. Variable values for each account are unpacked and read from
that JSON object. This should provide flexibility to this action.
