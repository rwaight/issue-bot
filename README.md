# Issue Bot
> GitHub Actions powered Issue Bot 🦾

<p align="center">
  <img src="https://github.com/imjohnbo/issue-bot/actions/workflows/ci.yml/badge.svg" />
  <img src="https://img.shields.io/github/license/imjohnbo/issue-bot" />
  <img src="https://img.shields.io/github/issues/imjohnbo/issue-bot" />
  <img src="https://img.shields.io/github/v/release/imjohnbo/issue-bot" />
</p>

## About

Work on a distributed team? Try using Issue Bot as a Scrum [standup](https://en.wikipedia.org/wiki/Stand-up_meeting) process automation bot to keep track of what you're all working on. 🤖

Have repeated tasks you're setting reminders for elsewhere? Issue Bot's got your back there, too. 👏

Or just need an issue created on a certain condition? Issue Bot is there when your CI build breaks. 💔

Issue Bot is a flexible GitHub action that takes care of a few issue related tasks:
- Opens new issue with `title`, `body`, `labels`, and `assignees`
- Uses [Mustache templating syntax](https://github.com/janl/mustache.js) in `body`, along with a couple of handy template variables: `assignees` and `previousIssueNumber`
- Closes most recent previous issue with all `labels` if `close-previous` is true
- Adds new issue to `project` (user, organization, or repository project based on value of `project-type`), `column`, and `milestone`
- Pins new issue and unpins previous issue if `pinned` is true
- Makes issue comments linking new and previous issues if `linked-comments` is true
- Assigns new issue only to the _next_ assignee in the list if `rotate-assignees` is true. Useful for duty rotation like first responder.
- Pairs well with [imjohnbo/extract-issue-template-fields](https://github.com/imjohnbo/extract-issue-template-fields) if you'd prefer to open issues based on [issue templates](https://docs.github.com/en/github/building-a-strong-community/about-issue-and-pull-request-templates#issue-templates)
- Supports Projects V2 (previously known as Projects Beta and Projects Next).

## v3 Migration
⚠️ If you're a `v2` user, please note that these breaking changes were introduced in `v3`: ⚠️
- `template` functionality has been moved to a separate action: https://github.com/imjohnbo/extract-issue-template-fields.
- `labels` now checks if **all** labels match. Before, it checked if **any** labels matched.

and these features were added 🎉:
- `project` and `column` for adding an issue to a [repository project board](https://docs.github.com/en/github/managing-your-work-on-github/about-project-boards).
- `milestone` for adding an issue to a [milestone](https://docs.github.com/en/github/managing-your-work-on-github/tracking-the-progress-of-your-work-with-milestones).

As always, your feedback and [contributions](#contributing) are welcome.

## Usage

Simple example:
```yml
# ...
- name: Create new issue
  uses: imjohnbo/issue-bot@v3
  with:
    assignees: "octocat, monalisa"
    title: Hello, world
    body: |-
      :wave: Hi, {{#each assignees}}@{{this}}{{#unless @last}}, {{/unless}}{{/each}}!
    pinned: true
# ...
```

For more examples, see a [GitHub-wide search](https://github.com/search?q=%22uses%3A+imjohnbo%2Fissue-bot%22&type=code) or [./docs/example-workflows](docs/example-workflows/):
- [Daily standup bot](docs/example-workflows/standup.yml)
- [Repeated tasks](docs/example-workflows/scheduled-task.yml)
- [Duty rotation](docs/example-workflows/first-responder.yml)
- [Ad hoc after broken CI build](docs/example-workflows/broken-build.yml)

## Inputs and outputs

See [action.yml](action.yml) for full description of inputs and outputs.

Generated by [`imjohnbo/action-to-mermaid`](https://github.com/imjohnbo/action-to-mermaid):

<!-- START MERMAID -->
```mermaid
flowchart LR
token:::optional-->action(Issue Bot Action):::action
title:::required-->action(Issue Bot Action):::action
body:::optional-->action(Issue Bot Action):::action
labels:::optional-->action(Issue Bot Action):::action
assignees:::optional-->action(Issue Bot Action):::action
project-type:::optional-->action(Issue Bot Action):::action
project:::optional-->action(Issue Bot Action):::action
column:::optional-->action(Issue Bot Action):::action
milestone:::optional-->action(Issue Bot Action):::action
pinned:::optional-->action(Issue Bot Action):::action
close-previous:::optional-->action(Issue Bot Action):::action
linked-comments:::optional-->action(Issue Bot Action):::action
linked-comments-new-issue-text:::optional-->action(Issue Bot Action):::action
linked-comments-previous-issue-text:::optional-->action(Issue Bot Action):::action
rotate-assignees:::optional-->action(Issue Bot Action):::action
action(Issue Bot Action)-->issue-number:::output
action(Issue Bot Action)-->previous-issue-number:::output
classDef required fill:#6ba06a,stroke:#333,stroke-width:3px
classDef optional fill:#d9b430,stroke:#333,stroke-width:3px
classDef action fill:blue,stroke:#333,stroke-width:3px,color:#ffffff
classDef output fill:#fff,stroke:#333,stroke-width:3px,color:#333
click token "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L9"
click title "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L15"
click body "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L20"
click labels "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L25"
click assignees "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L30"
click project-type "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L36"
click project "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L11"
click column "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L49"
click milestone "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L55"
click pinned "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L60"
click close-previous "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L67"
click linked-comments "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L74"
click linked-comments-new-issue-text "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L82"
click linked-comments-previous-issue-text "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L89"
click rotate-assignees "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L96"
click issue-number "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L104"
click previous-issue-number "https://github.com/imjohnbo/issue-bot/blob/main/action.yml#L107"
```
<!-- END MERMAID -->

## Template variables

The issue body is treated as a [Handlebars](https://handlebarsjs.com) template, with support for template variables:

- `assignees`: The array of issue assignees.
- `previousIssueNumber`: The previous issue number in the series.

The linked comments (`linked-comments-new-issue-text`, `linked-comments-previous-issue-text`) support these variables _and_:

- `newIssueNumber`: The new issue number.


## Projects V2 support:

Projects V2 were previously known as Projects Beta and Projects Next.

It works both for user owned projects and organization owned projects. Note that there are no repository owned projects V2.

To use it simply put project url:
```yaml 
projectV2: orgs/{name}/projects/{number}
projectV2: users/{name}/projects/{number}
```

e.g.
```
token: ...github_pat_token_with_project_scope
projectV2: orgs/github/projects/2
projectV2: users/octokit/projects/31
```

Please note that Projects V2 are still in beta and they require Github Personal Access Token (PAT) with full 'project' scope. You can set it via the `token:` field.


## Contributing

Feel free to open an issue, or better yet, a
[pull request](https://github.com/imjohnbo/issue-bot/compare)!

## License

[MIT](LICENSE)
