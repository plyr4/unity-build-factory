# Unity Build Factory

Unity Build Factory is a workflow for building and deploying multi-architecture Unity projects using [GitHub Actions](https://docs.github.com/en/actions) and [GameCI](https://game.ci/docs/github/getting-started/).
It was created to unify the repetitive build process for multiple projects and simplify deploying to GitHub Pages for private repositories with build artifacts that can be download by unauthenticated visitors.

By default the workflow will compile and build the Unity project for the following platforms:
- Windows
- Linux
- WebGL
- _**console support coming**_

It is capable of deploying the build artifacts to the following platforms:
- GitHub Pages
- _**steam support coming**_

I'm currently using Unity Build Factory to rapidly deploy demos for a few of my projects, like [Neebo](https://plyr4.github.io/unity-ufo/).

## Usage

1. Have a GitHub repository containing a Unity project.
1. Fork this repository and enable [Actions](https://docs.github.com/en/actions).
1. Create a GitHub [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) with read/write access to the target Unity project repository.
1. Review [GameCI](https://game.ci/docs/github/getting-started/) and add the following GitHub repository secrets to your fork:
   - `UNITY_EMAIL`
   - `UNITY_PASSWORD`
   - `UNITY_LICENSE`
   - `PAT_TOKEN`
1. In your fork, find the Actions workflow named  `project builder` and select `Run workflow` 
   1. enter the `owner/repo` corresponding to the target Unity project repository
   1. (optional) enter the `ref` or leave blank to use the repository's default branch
   1. (optional) toggle `deploy-to-pages` and set the `pages-deploy-branch`
   1. run the workflow

The workflow will likely take a long time to finish building the project on the first run but caching will speed up subsequent runs of the same project.

### Artifacts

The workflow will create a pre-release for each platform and attach the corresponding corresponding build that was produced in Actions. The pre-releases are updated with each run as to not clutter the repository with tags while testing and deploying numerous builds.

To download the built artifacts simply navigate to the releases section of the target repository and download the artifacts from the pre-release that corresponds to the platform you want to download.

### GitHub Pages
To enable support for deploying to GitHub Pages, the following steps are required:
1. Enable `repo:write` access for the GitHub [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) used by the workflow.
1. Set `Compression Format` to `Brotli` in the Unity project's [WebGL deploy settings](https://docs.unity3d.com/Manual/webgl-deploying.html).
1. Enable GitHub Pages for the target repository.
1. Add the following workflow to the target repository:
<details>
<summary><code>deploy-pages.yml</code></summary>

```yaml
name: deploy project to gh-pages

on:
  push:
    branches:
      - 'gh-pages'

permissions:
  contents: write
  pages: write
  id-token: write

concurrency:
  group: deploy
  cancel-in-progress: true

jobs:
  deploy:
    needs: read
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: configure pages
        uses: actions/configure-pages@v2
      - name: upload pages build artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: .
      - name: deploy pages
        id: deployment
        uses: actions/deploy-pages@v1
```
</details>

3. Add a WebGL build demo template to the target repository under the directory `WebGL`. 
      
   > A WebGL template in this context refers to a directory following the structure produced by Unity when running a WebGL build, 
   > the build factory will replace the template's game contents with what your project produces and keep the rest of the WebGL 
   > template as-is, allowing you to create a custom-tailored static site that hosts your game's WebGL build.

### Feedback

Open an issue: <https://github.com/plyr4/unity-factory/issues>

### License

See <https://www.gnu.org/licenses/>.
