### Git Repository Branch Promotion Validation

The Git repository must enforce a controlled branch promotion process to ensure that changes are merged into the environment branches in the correct order. The expected promotion flow is **Feature → DEV → SAT → UAT → SVP**.

Whenever a developer creates a Pull Request (PR), the pipeline must validate the source and target branches before allowing the PR to be merged. A feature branch must first be merged into the DEV branch. Changes can then be promoted from DEV to SAT, SAT to UAT, and UAT to SVP.

If a developer attempts to bypass an environment by directly creating a PR from a feature branch to SAT, UAT, SVP, or any other higher environment branch, the PR must fail validation. Similarly, a PR from DEV directly to UAT or from SAT directly to SVP must be rejected because it does not follow the defined promotion sequence.

The validation pipeline must identify the PR source branch and target branch and verify that the source branch is the immediately preceding branch in the promotion flow. If the source and target branches do not follow the approved sequence, the pipeline must return a failure status and display an appropriate error message explaining the required promotion path.

For example, if a developer creates a PR from `feature/ABC-123` directly to `SAT`, the pipeline must reject the PR and display a message such as:

> **Invalid promotion path. Changes must be merged into DEV before being promoted to SAT.**

The PR must be allowed to merge only when the promotion-order validation is successful. In addition to the Jenkins validation, branch protection rules must be configured in the Git repository to require the validation check to pass before merging. Direct pushes and unauthorized merges to protected environment branches should also be restricted.

This implementation will ensure that all changes follow the approved promotion path, prevent developers from skipping lower environments, and maintain consistency and control across the deployment pipeline.
