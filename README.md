# Easy GTSAM Tutorial

A bilingual (English / 한국어) tutorial that teaches what actually happens inside [GTSAM](https://gtsam.org/), built around **step-by-step scaffolding**: every chapter is sized to layer on the previous one, so that by the end deriving a `BetweenFactor` on a new Lie group feels like a routine exercise instead of a research project.

**Live site:** <https://limhyungtae.github.io/Easy_GTSAM_Tutorial/>

**What's covered**

| # | Chapter |
|---|---|
| Q | Quick Start (5-minute pose-graph SLAM) |
| 1 | `BetweenFactor` and how SLAM optimization actually works |
| 2 | SE(2): transformation matrix, Jacobian, block operations |
| 3 | Skew-symmetric matrix in 2D, the easy way |
| 4 | Unary factor: deriving a Lie-group Jacobian end-to-end |
| 5 | A worked example: `Rot2::unrotate` |
| 6 | `Pose2::BetweenFactor` Jacobian derivation |
| 7 | The Adjoint map, the easy way |
| 8 | `Pose3::BetweenFactor` Jacobian derivation |
| 9 | Kimera-PGMO's Deformation Factor derivation |
| 10 | Debugging factors with `numericalDerivative` |

**Audience.** You've run GTSAM on a toy SLAM problem and want to understand *why* its math is structured the way it is. If you only want to *run* pose-graph SLAM, Quick Start is enough; for everything else, the chapters scaffold you up to deriving your own factors.

**Author.** [Hyungtae Lim](https://limhyungtae.github.io/), Postdoctoral Associate at MIT SPARK Lab (Prof. Luca Carlone's group).

**Source content.** Each chapter is a re-organized English/Korean rendering of the author's original Korean blog posts at <https://limhyungtae.github.io/>. Chapter Q is condensed (with permission) from [@engcang's GTSAM tutorial](https://engcang.github.io/gtsam_tutorial.html).

**Stack.** Single static HTML page with a sticky left sidebar nav. KaTeX for math, Prism for syntax highlighting. No build step.

**Local preview**

```bash
cd docs && python3 -m http.server 8000
# open http://localhost:8000
```

**Deployment.** Pushes to `main` are deployed to GitHub Pages by `.github/workflows/pages.yml` (uploads the `docs/` folder as a Pages artifact). One-time setup: in the repo Settings → Pages, set **Source: GitHub Actions**. After that, every push to `main` redeploys automatically.

**PRs and issues welcome.** Especially: typo fixes, clearer math explanations, broken-link reports.
