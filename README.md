# game-release-uploader

A GitHub Actions workflow that downloads a torrent from a magnet link and uploads the files to a public Hugging Face dataset.

## Setup

1. Create a repo and put `release.yml` at `.github/workflows/release.yml`.
2. On Hugging Face, create an access token with **write** permission (Settings > Access Tokens).
3. In the GitHub repo, go to Settings > Secrets and variables > Actions and add a secret named `HF_TOKEN` with that token.

## Usage

1. Open the Actions tab and pick **Upload magnet to Hugging Face**.
2. Click **Run workflow**, check the magnet link, and enter the dataset as `username/name`. It is created as a public dataset if it doesn't exist yet.
3. When the run finishes, the files are at `https://huggingface.co/datasets/username/name`.

## How big torrents are handled

- The workflow first frees about 30 GB of disk on the runner, then groups the torrent's files into batches that fit. Each batch is downloaded, uploaded to Hugging Face and deleted before the next one starts, so a torrent larger than the disk still works.
- Files keep their original names, extensions and folder structure, so a single `.exe` stays one `.exe`.
- One limit remains: a single file has to fit on the runner's disk by itself (roughly 40 GB after cleanup). A bigger single file makes the run fail with a clear message.
- The run stops if the download stalls for 10 minutes, for example when nobody is seeding. A whole job is capped at 6 hours by GitHub.
- The dataset is public. Only upload files you have the right to distribute.
