 javascript-jest session notes

## Repo relationships

This repo (`cyber-dojo-languages/javascript-jest`) builds the Docker image.
The start-point files (source, tests, manifest) live in the partner repo:
`../../cyber-dojo-start-points/javascript-jest`

Development loop:
1. Edit `docker/Dockerfile.base` here
2. Run `./pipe_build_up_test.sh` — builds image, prints new tag at the end
3. Update `image_name` in `../../cyber-dojo-start-points/javascript-jest/start_point/manifest.json`
4. Edit start-point files in `../../cyber-dojo-start-points/javascript-jest/start_point/`
5. Run `../../cyber-dojo-start-points/javascript-jest/run_tests.sh` — verifies red/amber/green

**Important:** never run docker commands directly. Only test via `run_tests.sh`.
The runner containers have no internet access. The run_tests.sh script simulates three cyber-dojo [test] runs, once for red, once for amber, once for green, and prints a summary for each once that includes the duration. So you don't need to use the command time.

## Speed optimisation (2026-04-06)

Baseline red/amber/green times were ~6-7s each. Now ~1.5s each.

**What worked:** In `start_point/cyber-dojo.sh`, calling the tool binaries directly
(`node_modules/.bin/eslint`, `node_modules/.bin/jest`) instead of via `npm run`.
The `npm` process startup was almost entirely responsible for the overhead.

**What didn't help:** Removing `--coverage` from jest (~0s saving). Commenting out the
lint step (~0.5s saving). Both were negligible compared to the `npm run` overhead.
