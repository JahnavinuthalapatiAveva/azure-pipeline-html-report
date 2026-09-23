# azure-pipeline-html-report


Azure DevOps extension that provides a task for publishing report in a HTML format and embeds it into a Build and Release pages.

## AVEVA fork — Node 10 retirement remediation

This is AVEVA's fork of `JakubRumpca/azure-pipeline-html-report`, forked to
add `Node20_1`/`Node16` execution handlers (`PublishHtmlReport/task.json`)
and bump `azure-pipelines-task-lib` to `^4.13.0`, ahead of Azure DevOps
retiring Node10 task execution on 2026-11-24. Verified: `npm install` +
`node -e "require('./index.js')"` runs cleanly end-to-end under Node 20+.

Remaining steps before this can be packaged and installed:

1. Get a real Azure DevOps Marketplace publisher ID from
   https://marketplace.visualstudio.com/manage and replace
   `REPLACE_WITH_YOUR_PUBLISHER_ID` in `azure-devops-extension.json` and
   `dev_manifest.json`.
2. Build + package: `npm install && npm run build` (root), which runs
   `tfx extension create --manifest-globs azure-devops-extension.json
   --overrides-file dev_manifest.json` and produces a `.vsix`.
3. Upload/share the `.vsix` privately to your ADO organization at
   https://marketplace.visualstudio.com/manage (requires org-admin rights
   to install once shared).
4. Update the task references in the consuming pipeline templates (e.g.
   `aveva.components.build/Templates/CodeMetrics.yml`) from
   `JakubRumpca.azure-pipelines-html-report.PublishHtmlReport.PublishHtmlReport@1`
   to `<your-publisher>.aveva-codemetrics-html-report.PublishHtmlReport.PublishHtmlReport@1`.

### Extension

In order to see report on tab one must first use `Publish HTML Report` task. This is supporting task which makes html tab visible.

This task takes one parameter - required `reportDir` which is a path to report directory and also optional `tabName` which is the name of the tab displayed within Azure DevOps report. 
#### Example YAML setup

```YAML
steps:
  - task: PublishHtmlReport@1
    displayName: 'Publish HTML Report'
    inputs:
      reportDir: '$(ResultsPath)/reportName.html'
```