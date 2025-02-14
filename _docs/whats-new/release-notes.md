---
title: "Release Notes"
description: "New features, enhancements, and bug fixes"
group: whats-new
toc: true
---

Welcome to our unified Release Notes, including Codefresh pipelines and Codefresh GitOps!

## March 2023

### Features & Enhancements

#### CI/CD: Selective restart for failed build steps
We added the **Restart from a failed step** as an option to the pipeline's Policy settings, which you can enable/disable per pipeline. 
{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-mar23-restart-failed.png"
 url="/images/whats-new/rel-notes-mar23-restart-failed.png"
 alt="Restart from failed step option in Pipeline settings"
 caption="Restart from failed step option in Pipeline settings"
 max-width="50%"
%}

Previously, this option was available for all pipelines in the Builds page. Now, you can make it available according to the requirements of the specific pipeline. When disabled in the pipeline's settings, it is also disabled for that pipeline in the Builds page.

Why did we make this selective per pipeline?  
Because restarting from a failed step is not always the answer to the problem, especially as the pipelines restarts with the same state as before. 

If you have a failed Helm promotion step, and you updated the image, you would want the pipeline to use the new image. With the Restart option, the pipeline resumes execution at the same state as at the point of failure, never uses the updated image, and continues to fail.  

For details, see [Policy settings for pipelines]({{site.baseurl}}/docs/pipelines/pipelines/#policies) and [Restarting pipelines]({{site.baseurl}}/docs/pipelines/monitoring-pipelines/#restarting-the-pipeline).

<br>

#### CI/CD: Datadog integration enhancements
We enhanced our integration with Datadog to report additional information from Codefresh pipelines in Datadog. 
<br> 
The new information should make it even easier to monitor and analyze Codefresh pipelines in Datadog: 

* For manually triggered pipelines, the name of the user who initiated the pipeline.
* The Resumed field, if the pipeline was resumed after manual approval.
* The Parameters field with user-defined variables and Git parameters.
* Error messages for pipelines with errors.

{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-mar23-datadog.png"
 url="/images/whats-new/rel-notes-mar23-datadog.png"
 alt="Codefresh pipeline parameters in Datadog"
 caption="Codefresh pipeline parameters in Datadog"
 max-width="50%"
%}

See [Datadog pipeline integration]({{site.baseurl}}/docs/integrations/datadog/).

<br>



#### CI/CD: Override runtime environment for GitOps pipeline integrations
A GitOps pipeline integration uses the default runtime environment. After creating a GitOps pipeline integration, you can now override its runtime environment.
Codefresh uses the runtime environment for system actions such as Rollback.



{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-mar23-classic-gitops-runtime-env.png"
 url="/images/whats-new/rel-notes-mar23-classic-gitops-runtime-env.png"
 alt="Selecting runtime environment for GitOps pipeline integration"
 caption="Selecting runtime environment for GitOps pipeline integration"
 max-width="50%"
%}



<br>


#### GitOps: Deep links to applications
Codefresh supports adding Deep Links to third-party applications/platforms. Deep Links is an Argo CD feature that allows you to configure deep links to any third-party application/platform such as Splunk for example, to quickly redirect users to these applications/platforms. 

When configured, the Current State Tree view displays the linked-to applications in the resource's context menu.

{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-mar23-deep-links.png"
 url="/images/whats-new/rel-notes-mar23-deep-links.png"
 alt="Deep Links in application resource's context menu"
 caption="Deep Links in application resource's context menu"
 max-width="50%"
%}


For details, see [Configuring Deep Links to applications/resources for Hybrid GitOps]({{site.baseurl}}/docs/installation/gitops/monitor-manage-runtimes/#hybrid-gitops-configure-deep-links-to-applications--resources).

<br>

#### GitOps: Runtime name and namespace decoupled
We removed the dependency between the runtime name and namespace for Hybrid GitOps runtimes.
From CLI version v0.1.41, and runtime version: v0.1.27, you can have different names for runtimes and namespaces. 

<br>

#### GitOps Apps dashboard usability enhancements

* **New icons for health status**  
  The color-coded borders around application resources that indicated their health status were not the most intuitive and easy to identify.  
  We listened to your feedback, and replaced them with a set of icons affixed to the resource type:<br>
  {::nomarkdown}<img src="../../../images/icons/current-state-healthy.png" display=inline-block"> Healthy, <img src="../../../images/icons/current-state-progressing.png" display=inline-block"> Progressing, <img src="../../../images/icons/current-state-suspended.png" display=inline-block"> Suspended, <img src="../../../images/icons/current-state-missing.png" display=inline-block"> Missing, <img src="../../../images/icons/current-state-degraded.png" display=inline-block/> Degraded, and <img src="../../../images/icons/current-state-unknown.png" display=inline-block/> Unknown.
  {:/}     


* **Filter by K8s labels**  
  Also in the GitOps Apps dashboard, you can filter applications by Kubernetes labels.  

{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-mar23-label-filters-apps.png"
 url="/images/whats-new/rel-notes-mar23-label-filters-apps.png"
 alt="Filter applications by Kubernetes labels"
 caption="Filter applications by Kubernetes labels"
 max-width="40%"
%}

See [Monitoring GitOps applications]({{site.baseurl}}/docs/deployments/gitops/applications-dashboard/).  

<br>

### Bug fixes

#### CI/CD  
* Triggers for inactive webhooks return 200.
* In full-screen view mode, the pipeline list panel on the left overlaps the pipeline YAML.
* Unable to override image used for `cf-runtime-lv-monitor Daemonset` in the Runner Helm chart. 
* Adding an annotation with the same name as an existing annotation, replaces the existing annotation instead of showing an error.
* CVE in `cf-runtime-volume-cleanup` .
* Builds terminated on prolonged inactivity.
* Opening build deleted by retention policy shows pop-up for switching accounts: `Build is from a different account: <account>. To view this build, you must switch accounts`.
* Unable to edit Inline YAML when returning to the Workflow tab and switching from `Use YAML from repository` to `Inline YAML`. 
* Parallel steps with kubectl-related operations result in `error: open /codefresh/volume/sensitive/.kube/config.lock: file exists`.
* Step member variables not supported in parallel steps. 
* Runtime monitor start failure after upgrading to EKS 1.21.
* For Bitbucker Server, Commit trigger that deleted branches results in failed builds.
* `image-enricher` step breaks with error `Failed to assign pull request 9531 to your image ... reason Cannot read property 'map' of undefined`.
* (On-premises only) Enabling `forbidDecrypt` Feature Flag breaks `github-release` step.
* (On-premises only) UI logs not available with on-premises release version 1.3.9.
* (On-premises only) Creating account via Terraform results in plugin error. 

<br>

#### GitOps  
* Rollout not set to Paused on inconclusive analysis run.
* Rollout metrics not displayed for steps in the Rollout player.
* Expanded analysis step in Rollout Player minimizes automatically. 
* Adding a Helm application with two or more `values` files results in missing value errors.
* Unable to return to account in GitOps module after impersonating a user.
* Invalid specification for default Git integration.

<br><br>

## February 2023

### Features & Enhancements

#### CI/CD: Slack integration notification for builds terminated by system
Notifications for failed builds are equally, if not more important, than those for successful builds. Getting notifications for system-terminated builds is crucial, as it indicates that the build was stopped because of pipeline policy and may require immediate attention. You can quickly investigate the issue and take corrective action if necessary.


We added a new option to our Slack integration to notify you whenever builds are terminated by the system. 

{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-feb23-slack-failed-notification.png"
 url="/images/whats-new/rel-notes-feb23-slack-failed-notification.png"
 alt="Slack notification option for system-terminated builds"
 caption="Slack notification option for system-terminated builds"
 max-width="50%"
%}

Here's an example of the notification you would receive in Slack.

{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-feb23-terminate-build-slack-example.png"
 url="/images/whats-new/rel-notes-feb23-terminate-build-slack-example.png"
 alt="Example Slack notification for system-terminated builds"
 caption="Slack notification for system-terminated builds"
 max-width="50%"
%}

<br>

#### CI/CD: Multiple Helm contexts for pipelines
With support for multiple Helm registry contexts in the same pipeline, dependencies in any of the imported Helm registry contexts in the Helm chart are automatically authenticated and added.
For the Helm `install` and `push` actions, you can select the primary Helm registry context for the command.
For details, see [Import Helm configurations into your pipeline definition]({{site.baseurl}}/docs/deployments/helm/using-helm-in-codefresh-pipeline/#step-4-optional-import-helm-configurations-into-your-pipeline-definition) and [Action modes]({{site.baseurl}}/docs/deployments/helm/using-helm-in-codefresh-pipeline/#helm-step-action-modes).


<br>

#### CI/CD: Multiple cache sources for pipeline builds

Docker has support for specifying external cache sources for builds. We added the `cache-from` argument to our `build` step allowing you to specify additional cache sources and speed up the build process. Multiple cache sources are useful when your primary cache source is unavailable or slow.

Here's an example of `cache-from` with `buildkit`:

{% highlight yaml %}
{% raw %}
version: '1.0'
steps:
  BuildMyImage:
    title: Building My Docker image
    type: build
    image_name: my-app-image
    dockerfile: my-custom.Dockerfile
    tag: 1.0.1
    buildkit: true
    build_arguments:
    - BUILDKIT_INLINE_CACHE=1
    cache_from:
    - my-registry/my-app-image:${{CF_BRANCH}}
    - my-registry/my-app-image:master
{% endraw %}         
{% endhighlight %}

For details, see [`cache_from` in `build` step fields]({{site.baseurl}}/docs/pipelines/steps/build/#fields).

<br>

#### CI/CD: Control thresholds for memory usage warning banner
Remember the banner that alerted you whenever the memory usage for a pipeline build exceeded 70 or 90%?
You can now decide the usage threshold at which to display the banner. Increasing the threshold helps avoid premature warnings for pipelines that do not consume a lot of memory, while decreasing it for resource-intensive pipelines helps avoid build failures. 

As part of the account-level configuration settings for pipelines (**Toolbar Settings > Pipeline Settings**), you can decide to display the banner when memory usage exceeds 70%, 90% (as before), or the actual limit of 100% (new option). 


{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-feb23-build-memory-warning.png"
 url="/images/whats-new/rel-notes-feb23-build-memory-warning.png"
 alt="Options for memory usage limit warning banner"
 caption="Options for memory usage limit warning banner"
 max-width="50%"
%}

Users can then override the memory-usage threshold for individual pipelines.
See [Memory usage warning for pipeline builds]({{site.baseurl}}/docs/pipelines/configuration/pipeline-settings/#memory-usage-warning-for-pipeline-builds).


<br>

#### CI/CD: New flow for Cloud Builds for pipelines
Previously, all Codefresh accounts had access to a SaaS runtime environment to run pipelines. However, this is no longer the case. Account administrators can request SaaS runtime environments by clicking **Enable Cloud Builds** in Codefresh. This action triggers an email request to Codefresh, and you should receive a response within 24 hours.


<br>

#### GitOps: Upgrade to Argo CD 2.6
We have upgraded the Argo CD version for our GitOps module to v2.6. 
For details, see [Argo CD Releases](https://github.com/argoproj/argo-cd/releases){:target="\_build"}.


<br>

#### Usability enhancements
Saves time and ease of use in interactions with Codefresh.  

* **CI/CD: Prompt to switch accounts**  
  To avoid confusion, when you are signed into more than one Codefresh account, you are prompted to either switch to the active account or return to the previous one. 

{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-feb23-switch-accnt-prompt.png"
 url="/images/whats-new/rel-notes-feb23-switch-accnt-prompt.png"
 alt="Switch active account prompt"
 caption="Switch active account prompt"
 max-width="50%"
%}

* **CI/CD: Case-insensitive search for Pipelines and Pipeline List view**
  Search is now easier as queries are case-insensitive. 

* **GitOps: Terminate Sync now in application header**
We moved the **Terminate Sync** button from the Sync details drawer to the application header making it easy to terminate problematic sync operations if you need to. 


{% include
 image.html
 lightbox="true"
 file="/images/whats-new/rel-notes-feb23-terminate-sync-app-header.png"
 url="/images/whats-new/rel-notes-feb23-terminate-sync-app-header.png"
 alt="Terminate sync option in Application Header"
 caption="Terminate sync option in Application Header"
 max-width="50%"
%}

See [Application header]({{site.baseurl}}/docs/deployments/gitops/applications-dashboard/#get-status-from-application-header) in [Monitoring GitOps applications]({{site.baseurl}}/docs/deployments/gitops/applications-dashboard/).



<br>

### Bug fixes

#### CI/CD
- Logs not generated and slow build execution.
- `CF_HELM_SET` variable  printed as [object Object].
- Variables added via pipeline hooks not rendered for build annotations.
- Build does not fail on error for `when` condition.
- Clicking a badge in Pipeline > General Settings results in an error.
- When cloning pipelines, **Copy YAML from** drop-down does not display all pipelines in project, if project has more than 100 pipelines.  
- Running pipeline locally results in error: " checkAvailabilityWithRetry error: ..., connect EACCES /var/run/docker.sock .. " 
- Tooltips not displayed on hover over Usage Report columns.
- Engine containers left in Docker after `codefresh run --local`.
- (On-premises only) Liveness probe failures on `cf-api` pods.
- (On-premises only) Tooltip on hover over build/project names in the Builds page, shows _topbar.title_ instead of the build/project name.


<br>

#### GitOps
- Trying to recover runtime results in "invalid memory address or nil pointer reference" error.
- Rollout rollback failure when rollout namespace is different from application namespace.
- Clicking native Argo Workflows link displays empty screen. 