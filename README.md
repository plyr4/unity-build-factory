# Unity Factory

This project contains a workflow for compiling and building multi-architecture Unity projects using [GitHub Actions](https://docs.github.com/en/actions) and [GameCI](https://game.ci/docs/github/getting-started/).

## Usage

1. Have a GitHub repository containing a Unity project.
1. Fork this repository and enable [Actions](https://docs.github.com/en/actions).
1. Review [GameCI](https://game.ci/docs/github/getting-started/) and add the following GitHub repository secrets to your fork:
   - `UNITY_EMAIL`
   - `UNITY_PASSWORD`
   - `UNITY_LICENSE`
1. Create a GitHub Personal Access Token with read/write access to the target Unity project repository and add `PAT_TOKEN` to GitHub repository secrets.
1. Find the Actions workflow for  `project builder` and select `Run workflow` 
   1. enter the `<owner>/<repo>` corresponding to the target Unity project repository
   1. toggle `deploy-to-pages` to enable pushing to `gh-pages` branch of the target repo
   1. run the workflow

By default the workflow will compile and build the Unity project for the following platforms:
- Windows
- Linux
- WebGL
- *console support coming soon?*

The workflow will likely take a long time to complete on the first run, but caching should speed up subsequent runs of the same project.

## Contact

Open an issue at <https://github.com/plyr4/unity-factory/issues>

### License

Copyright (C) 2024 David Vader

This program is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License along with this program. If not, see <https://www.gnu.org/licenses/>.