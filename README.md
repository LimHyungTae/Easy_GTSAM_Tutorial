# How GTSAM Works · A Bilingual Internals Tutorial

A self-contained, bilingual (English / 한국어) tutorial site explaining what actually happens inside [GTSAM](https://gtsam.org/) when you call `BetweenFactor`, `evaluateError`, `optimize()`, and friends.

**Live site:** <https://limhyungtae.github.io/gtsam/>

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

**Audience.** You've run GTSAM on a toy SLAM problem and want to understand *why* its math is structured the way it is. If you only want to *run* pose-graph SLAM, Quick Start is enough; for the rest, see the chapters above.

**Author.** [Hyungtae Lim](https://limhyungtae.github.io/), Postdoctoral Associate at MIT SPARK Lab (Prof. Luca Carlone's group).

**Source content.** Each chapter is a re-organized English/Korean rendering of the author's original Korean blog posts at <https://limhyungtae.github.io/>. Chapter Q is condensed (with permission) from [@engcang's GTSAM tutorial](https://engcang.github.io/gtsam_tutorial.html).

**Stack.** Single static HTML page with sticky left sidebar navigation. KaTeX for math, Prism for syntax highlighting. No build step.

**Local preview**

```bash
cd docs && python3 -m http.server 8000
# open http://localhost:8000
```

**PRs and issues welcome.** Especially: typo fixes, clearer math explanations, broken-link reports.
