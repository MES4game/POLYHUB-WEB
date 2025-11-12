# POLYHUB-WEB

Copyright (c) 2025 Maxime DAUPHIN, Maël HOUPLINE, Julien TAP. All rights reserved.
Licensed under the MIT License. See LICENSE file for details.

Root repository of project "PolyHUB" for ET4 web project.

This repository contains only installation, configuration and documentation files.

Front-end and back-end code are in separate repositories:
- Front-end: https://github.com/MES4game/POLYHUB-WEB-FRONT
- Back-end: https://github.com/MES4game/POLYHUB-WEB-BACK

---

## Contents

- [Structure](#structure)
- [Git conventions](#git-conventions)
  - [Branches](#branches)
  - [Commits](#commits)
  - [PR](#pr)
- [Git Commands](#git-commands)
- [GitHub](#github)
  - [Settings](#settings)
    - [General](#general)
    - [Rules](#rules)
    - [Actions](#actions)
    - [Environments](#environments)
    - [Advanced Security](#advanced-security)

---

## Structure

- `.github`: GitHub-related files (workflows, issue templates, etc.)
- `.gitignore`: files to ignore by git
- `LICENSE`: license file (MIT)
- `polyhub-web-db_schema.svg`: database schema diagram
- `rapport.md`: project report
- `README.md`: this file
- `site.conf.template`: nginx configuration template

---

## Installation

- NO need to clone any repository

- prerequisites:
  - Docker
  - Docker Compose
  - Nginx (optional, only if you want to use it as reverse proxy)

- images:
  - front image: [![Docker Image Version](https://img.shields.io/docker/v/mes4game/polyhub-web-front?sort=semver)![Docker Image Size](https://img.shields.io/docker/image-size/mes4game/polyhub-web-front?sort=semver)](https://hub.docker.com/r/mes4game/polyhub-web-front)
  - back image: [![Docker Image Version](https://img.shields.io/docker/v/mes4game/polyhub-web-back?sort=semver)![Docker Image Size](https://img.shields.io/docker/image-size/mes4game/polyhub-web-back?sort=semver)](https://hub.docker.com/r/mes4game/polyhub-web-back)
  - db image: [![Docker Image Version](https://img.shields.io/docker/v/mes4game/polyhub-web-db?sort=semver)![Docker Image Size](https://img.shields.io/docker/image-size/mes4game/polyhub-web-db?sort=semver)](https://hub.docker.com/r/mes4game/polyhub-web-db)

- steps to install:
  1. open a terminal and navigate to the folder where you want to download the `docker-compose.yml` file
  2. you can find an example of `docker-compose.yml` [here](./docker-compose.yml)
      - you can copy it with `curl -o docker-compose.yml https://raw.githubusercontent.com/MES4game/POLYHUB-WEB/main/docker-compose.yml`
  3. you also need to copy [`example.env`](./example.env) to `.env`
      - you can get it with `curl -o .env https://raw.githubusercontent.com/MES4game/POLYHUB-WEB/main/example.env`
  4. edit `.env` to your needs
  5. you also need to copy [`example.config.json`](./example.config.json) in the folder you mount for the volume used in container with the name `config.json`
      - you can get it with `curl -o /path/to/mounted-front/config.json https://raw.githubusercontent.com/MES4game/POLYHUB-WEB/main/example.config.json`
  6. edit `config.json` to your needs
  5. run `docker compose up -d --force-recreate` to start the containers
  6. (optional) if you want to use Nginx as reverse proxy, copy `site.conf.template` as `polyhub.example.com.conf` to your Nginx configuration folder and edit it to your needs
      - you can get it with `curl -o /etc/nginx/sites-available/polyhub.example.com.conf https://raw.githubusercontent.com/MES4game/POLYHUB-WEB/main/site.conf.template`
      - create a symlink to `sites-enabled` (e.g. `ln -s /etc/nginx/sites-available/polyhub.example.com.conf /etc/nginx/sites-enabled/`)
      - restart Nginx to apply the changes (e.g. `systemctl restart nginx`)

---

## Git conventions

- ### Branches
  - `main`: production branch
  - `dev`: development branch (all features must be merged here first)
  - `dev-<id>`: task branches (must be created from `dev` and merged back to `dev`)

- ### Commits
  - `<type>(<scope>): <comment>`
    - `<type>`: type of the commit (must be one of the following, can be combined with `|`):
      - `feat`: new feature
      - `fix`: bug fix
      - `docs`: documentation only changes
      - `style`: changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
      - `refactor`: code change that neither fixes a bug nor adds a feature
      - `perf`: code change that improves performance
      - `test`: adding missing tests or correcting existing tests
      - `chore`: changes to the build process or auxiliary tools and libraries such as documentation generation
      - `merge`: merges branches
    - `<scope>`: name of the branch
    - `<comment>`: short description of the commit
  - example: `feat(dev-42): add login feature`
  - example: `feat|perf|style(dev-21): add last event feature, improve performance for render and fix styles of first section`
  - every commit must be signed with a GPG/SSH key
  - make reasonable sized commits (do not commit everything in one commit, but do not make too many small commits either)
  - write meaningful commit messages (do not use `fix` or `update` as comment, be more specific)

- ### PR
  - title: `merge(<source> -> <target>): <comment>`
    - `<source>`: source branch
    - `<target>`: target branch
    - `<comment>`: short description of the PR
  - description
    - optional: follow the template provided by GitHub
    - if you did not use the template, at least provide a description of the changes
  - for the merge commit, let GitHub generate it automatically (do not edit it)
  - reviewers: at least one reviewer must be assigned (preferably the repository admin or the service manager)
  - In case of a release PR (from `dev` to `main`):
    - title: `r/<version> - <name>`
      - `<version>`: version of the release (must follow [semantic versioning](https://semver.org/), e.g. `1.2.3`)
      - `<name>`: name of the release
    - description:
      - follow the template provided by GitHub
      - if you did not use the template, at least provide a description of the changes
      - you can omit the "Changelog" section
      - do not use "---" in the PR description, it breaks the formatting, only let the one by default
      - a link to compare the changes since last release will be automatically generated by GitHub in the release description

---

## Git Commands

- `git status`: check the status of the repository
- `git fetch origin`: fetch changes from remote
- `git checkout <branch>`: switch to an existing branch
- `git pull origin`: pull changes from remote branch
- `git rebase origin/<branch>`: rebase current branch onto remote branch, if you want to get latest changes of another branch (must be done after `git fetch origin`)
- `git add .`: stage all changes
- `git add <file>...`: stage specific file (can use glob patterns)
- `git commit -m "<commit message>"`: commit staged changes with a message
- `git push origin`: push changes to remote branch
- `git stash -u`: stash changes (to be reapplied later, for example when switching branch)
- `git stash pop`: reapply stashed changes

---

## GitHub

- ### Settings
  - #### [General](https://github.com/MES4game/POLYHUB-WEB/settings)
    - `Default branch`: `main`
    - `Releases`:
      - `Enable release immutability`: true
    - `Features`:
      - `Wiki`: false
      - `Issues`: false
      - `Sponsorships`: false
      - `Preserve this repository`: true
      - `Discussions`: false
      - `Projects`: false
    - `Pull Requests`:
      - `Allow merge commits Loading`: true
        - `Default commit message`: Pull request title and description
      - `Allow squash merging`: false
      - `Allow rebase merging`: false
      - `Always suggest updating pull request branches`: true
      - `Allow auto-merge`: false
      - `Automatically delete head branches`: false
    - `Pushes`:
      - `Limit how many branches and tags can be updated in a single push`: 2

  - #### [Rules](https://github.com/MES4game/POLYHUB-WEB/settings/rules)
    - `no branches`:
      ```json
      {
        "name": "no branches",
        "target": "branch",
        "source_type": "Repository",
        "enforcement": "active",
        "conditions": {
          "ref_name": {
            "exclude": [
              "refs/heads/main",
              "refs/heads/dev",
              "refs/heads/dev-*"
            ],
            "include": [
              "~ALL"
            ]
          }
        },
        "rules": [
          {
            "type": "deletion"
          },
          {
            "type": "non_fast_forward"
          },
          {
            "type": "update"
          },
          {
            "type": "creation"
          }
        ],
        "bypass_actors": []
      }
      ```
    - `protected branches`:
      ```json
      {
        "name": "protected branches",
        "target": "branch",
        "source_type": "Repository",
        "enforcement": "active",
        "conditions": {
          "ref_name": {
            "exclude": [],
            "include": [
              "refs/heads/main",
              "refs/heads/dev"
            ]
          }
        },
        "rules": [
          {
            "type": "deletion"
          },
          {
            "type": "non_fast_forward"
          },
          {
            "type": "creation"
          },
          {
            "type": "pull_request",
            "parameters": {
              "required_approving_review_count": 1,
              "dismiss_stale_reviews_on_push": true,
              "require_code_owner_review": true,
              "require_last_push_approval": false,
              "required_review_thread_resolution": true,
              "automatic_copilot_code_review_enabled": false,
              "allowed_merge_methods": [
                "merge"
              ]
            }
          }
        ],
        "bypass_actors": []
      }
      ```
    - `dev branches`:
      ```json
      {
        "name": "task branches",
        "target": "branch",
        "source_type": "Repository",
        "enforcement": "active",
        "conditions": {
          "ref_name": {
            "exclude": [],
            "include": [
              "refs/heads/dev-*"
            ]
          }
        },
        "rules": [
          {
            "type": "deletion"
          },
          {
            "type": "non_fast_forward"
          },
          {
            "type": "required_signatures"
          }
        ],
        "bypass_actors": []
      }
      ```
    - `no tags`:
      ```json
      {
        "name": "no tags",
        "target": "tag",
        "source_type": "Repository",
        "enforcement": "active",
        "conditions": {
          "ref_name": {
            "exclude": [
              "refs/tags/v*.*.*"
            ],
            "include": [
              "~ALL"
            ]
          }
        },
        "rules": [
          {
            "type": "deletion"
          },
          {
            "type": "non_fast_forward"
          },
          {
            "type": "creation"
          },
          {
            "type": "update"
          }
        ],
        "bypass_actors": []
      }
      ```

  - #### [Actions](https://github.com/MES4game/POLYHUB-WEB/settings/actions)
    - `Actions permissions`: Allow all actions and reusable workflows
    - `Require actions to be pinned to a full-length commit SHA`: false
    - `Approval for running fork pull request workflows from contributors`: Require approval for all external contributors
    - `Workflow permissions`: Read repository contents and packages permissions
    - `Allow GitHub Actions to create and approve pull requests`: false

  - #### [Environments](https://github.com/MES4game/POLYHUB-WEB/settings/environments)
    - none

  - #### [Advanced Security](https://github.com/MES4game/POLYHUB-WEB/settings/security_analysis)
    - `Dependency graph`: Enable
      - `Automatic dependency submission`: Enabled
    - `Dependabot`:
      - `Dependabot alerts`: Enable
    - `Protection rules`:
      - `Check runs failure threshold`:
        - `Security alert severity level`: High or higher
        - `Standard alert severity level`: Only errors
    - `Secret Protection`: Enable
    - `Push protection`: Enable
