name: Profile Metrics

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  metrics:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          user: priyamshrivastava
          template: classic
          base: header, activity, community, repositories, metadata
          plugin_languages: yes
          plugin_languages_analysis_timeout: 15
          plugin_lines: yes
          plugin_lines_history_limit: 30
          config_timezone: Asia/Kolkata
          filename: metrics.svg

  snake:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Generate snake game from GitHub contribution grid
        uses: Platane/snk@v3
        with:
          github_user_name: priyamshrivastava
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push snake animation to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
