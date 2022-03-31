## Basic description
Given a version number MAJOR.MINOR.PATCH, increment the:
- MAJOR version when you make incompatible API changes,
- MINOR version when you add functionality in a backwards compatible manner, and
- PATCH version when you make backwards compatible bug fixes.

## Commits prefixes
- feat: new feature
- fix: bug fix
- refactor: refactoring production code
- style: formatting, missing semicolons, etc.; no code change
- docs: changes to documentation
- test: adding or refactoring tests no production code change
- chore: updating dependencies etc. no production code change

### Notes
- Major version zero (0.y.z) is for initial development. Anything MAY change at any time. The public API SHOULD NOT be considered stable.
