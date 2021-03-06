page_main_title: Single pane of glass
main_section: Provision
sub_section: Before you start
page_title: Single Pane of Glass (SPOG) View
page_description: Features of Single Pane of Glass (SPOG) View

# Single Pane of Glass (SPOG) View

The Single Pane of Glass (SPOG) view is a real time, interactive, visual representation of all pipelines across your organization.

Each job in this view will update colors in real time based on the status of the job:

- Green if the last run was successful
- Red if the last run failed
- Blue if the job is in progress
- Grey if the job is queued and waiting for available capacity

<img src="/images/getting-started/spog.png" alt="SPOG: Single pane of glass">

You can interact with the SPOG view in the following ways:

- [Zoom in](#zoom) and out to see additional details for any part of the pipeline
- [Run or pause the job](#manual-build)
- [View console output](#console) and additional meta-data for each version of the job.
- [Trace upstream meta-data](#trace)
- [View in tabular format](#grid-view)
- [Filter with flags](#filter)
- [View orphaned and soft-deleted resources](#orphan)

<a name="zoom"></a>
##Zoom

The SPOG view is meant to give you one dashboard where you can have a macro view across all pipelines in your organization. However, you might want to focus on specific sections of your pipeline in some cases, like seeing a 'red' job in the pipeline for example.

You can zoom in or out of any part of the pipeline with your mouse scrollbar or with pinch-to-zoom on your mouse keypad. A zoomed representation of the pipeline is shown below:

<img src="/images/getting-started/spog-zoom.png" alt="SPOG: Single pane of glass">

<a name="manual-build"></a>
##Run/pause job

You can right click on any job to trigger a manual run or pause the job.

<img src="/images/getting-started/spog-right-click-menu.png" alt="Run or pause a job">

Please note that if you choose to trigger a manual run, the job will run with the latest input values available to it.

Pausing a job turns off automatic triggers for the job. You can continue triggering paused jobs manually.

<a name="console"></a>
##View console output

You can click on any job in your pipeline to view console logs.

<img src="/images/getting-started/spog-console.png" alt="View console logs for a job">

Please note that in most cases, the logs you are interested in will be under the **Executing Task** -> **/home/shippable/micro/nod/stepExec/managed/<job type>/run.sh** section. This section is shown as expanded in the screenshot above.

In addition to viewing logs for the latest run, you can also view logs for historical runs by choosing a past run in the UI.

<a name="trace"></a>
##Tracing upstream events and data

The first question people tend to ask when their application breaks is: what changed? Typically, the more complicated a pipeline gets, the more difficult this question becomes to answer.  To make this easy on the user, Shippable has embedded trace functionality in every pipeline job. It allows you to pick any particular job in your pipeline, and view the entire tree of events that led up to that particular version of that particular job with just a few clicks.  This allows you to go all the way back to the initial code commit that triggered the workflow to begin with.

The trace view shows the inputs to a given job. To get to the trace view:

- Click on a job in the SPOG to open the console.
- Click the **Trace** tab above the console. This opens a list of the job's inputs. You can drill down to see the versions of the inputs and all data for that version.

For example, let's assume your [deploy job](/platform/workflow/job/deploy/) just ran and you want to see what was deployed. When you click **Trace**, the list shows the inputs to that deploy job:

<img src="/images/deploy/deploy-trace.png" alt="majestic trace tab">

In the image above, the deploy job has two inputs: a [manifest job](/platform/workflow/job/manifest/) and a [cluster resource](/platform/workflow/resource/cluster/). You can click on the image resource in the manifest to see which tag was deployed.

<a name="grid-view"></a>
##View in tabular format

The default view of your SPOG is graphical. However, you might want to see all jobs are resources in a table in order to quickly find a particular job or resource.

You can switch between table and graphical format by clicking on the Grid View button on your SPOG:

<img src="/images/getting-started/spog-grid-view.png" alt="View SPOG in tabular format">

You can take some actions on the jobs in grid view, like running or canceling builds, soft or hard deleting resources and jobs, or viewing console logs for jobs.

<a name="filter"></a>
##Filter SPOG

If you have a lot of pipelines configured, the SPOG view can get busy and it can be difficult to find a specific job or resource. You can use the `flags` keyword to filter SPOG for easier readability.

The `flags` keyword can be set in any job or resource:

```
jobs:
  - name: <string>
    type: <job type>
    #other job related inputs
    flags:
      - <filter name>
```

Once you commit this, the filters will be shown in the **Filter** dropdown in your SPOG view. You can choose one or more filters as shown below:

<img src="/images/getting-started/filter-spog.png" alt="Filtering SPOG">

Please note that you do not need to add flags to every job or resource in your pipeline. If we detect flags for a job or resource and you filter SPOG with that flag, the UI will display all upstream and downstream jobs and resources in the pipeline.

<a name="orphan"></a>
##View orphaned and soft-deleted resources

Orphaned resources are resources that are defined in your `shippable.yml` but not used as inputs or outputs for any job.
Soft-deleted resources and jobs are resources/jobs that are deleted from your `yml` files. We do not automatically delete these to avoid situations where something gets mistakenly deleted from the yml and you want to retrieve it at a later time.

To view either or both of these types of resources, click on the arrow dropdown in your SPOG pane header and choose the ones you want to view.

<img src="/images/getting-started/spog-orphan-delete.png" alt="View orphaned or deleted resources">

<a name="dry-run"></a>
##Testing your pipeline config

Dry Run feature allows you to validate and test your syncRepo pipelines before actually adding them. Currently we support dry-run pipelines for branches only, dry-run pipelines for Pull Requests is work in progress.

To check dry run pipeline for a branch, goto subscriptions pipeline tab. On the right, click
on **Dry Run** button. Search and select the project for which you want to check dry run pipeline.

<img src="/images/getting-started/dry-run-search-project.png" alt="search project" style="width:700px;"/>

After selecting the project, you should select a branch for which you whould like to view dry run pipeline.

<img src="/images/getting-started/dry-run-search-branch.png" alt="search branch" style="width:700px;"/>

After selecting the branch, your dry run pipeline will show up. It will look exactly same as how it will look if you add an actual syncRepo for selected project and branch.
Any errors or warnings will be shown to you so that you can correct them before adding syncRepo.
<img src="/images/getting-started/dry-run-pipeline.png" alt="dry run pipeline" style="width:700px;"/>

To check the dry run pipeline for any other project or branch, you can select the same from dropdowns and dry run pipelines for those will be shown.

Please note that, since we support resource sharing between syncRepos in same subscription, if any step(IN, OUT or NOTIFY) resources or jobs for a job are not present in current syncRepo but are present in other syncRepo in the same subscription then those resources or jobs
will be shown in dry run pipeline.
