---
title: Publish Site Does Not Publish All Items with the Sitecore Publishing Service
tags: [Sitecore, Publishing Service]
sharing:
  linkedin: One thing to try when a publish site does not publish all items with the Sitecore Publishing Service
---

Today, I got a weird problem with the Sitecore Publishing Service 3.1.1. This blog post is there for future reference and to help others in the same situation.

<!-- more -->

## Problem

Some items were not published by the Sitecore Publishing Service when I did a Publish Site operation.

## Timeline

1. I did a clasic Sitecore site smart publish in all languages on my HabitatHome demo instance (Sitecore 9 + Sitecore Experience Comemrce 9 + SPE + SXA + Many other modules + demo content = more than 100,000 items).
2. After 3 hours and 15 minutes publishing more than 105,000 items, IIS decided it was time for the routine application pool recycle. Sitecore shutdowned with the message `HostingEnvironment initiated shutdown`.
3. I got angry and decided to install the Sitecore Publishing Service and module version 3.1.1.
4. I did a publish site in all languages. It took only 5 seconds! I was amazed. However, I did not check at that time, but it affected only 12 items.
5. The published demo site that was supposed to work after publishing was still not working.
6. To check if the publish worked correctly, I opened the Sitecore Desktop, switched to the web database, and opened the Content Editor. It displayed an exception so I was not able to use it.
    ```
    Server Error in '/' Application.

    Object reference not set to an instance of an object.

    Description: An unhandled exception occurred during the execution of the current web request. Please review the stack trace for more information about the error and where it originated in the code.

    Exception Details: System.NullReferenceException: Object reference not set to an instance of an object.

    Source Error:
    An unhandled exception was generated during the execution of the current web request. Information regarding the origin and location of the exception can be identified using the exception stack trace below.

    Stack Trace:
    [NullReferenceException: Object reference not set to an instance of an object.]
      Sitecore.Commerce.XA.Foundation.Upgrade.Services.UpgradeService.get_ValidUpgradeScripts() +155
      Sitecore.Commerce.XA.Foundation.Upgrade.Pipelines.ContentVersionWarning.Process(GetContentEditorWarningsArgs args) +156
      (Object , Object ) +8
      Sitecore.Pipelines.CorePipeline.Run(PipelineArgs args) +483
      Sitecore.Pipelines.DefaultCorePipelineManager.Run(String pipelineName, PipelineArgs args, String pipelineDomain, Boolean failIfNotExists) +235
      Sitecore.Pipelines.DefaultCorePipelineManager.Run(String pipelineName, PipelineArgs args, String pipelineDomain) +21
      Sitecore.Shell.Applications.ContentManager.Editor.GetWarnings(Boolean hasSections) +323
      Sitecore.Shell.Applications.ContentManager.Editor.Render(RenderContentEditorArgs args, Control parent) +158
      Sitecore.Shell.Applications.ContentManager.ContentEditorForm.RenderEditor(Item item, Item root, Control editorsContainer, Boolean showEditor) +290
      Sitecore.Shell.Applications.ContentManager.ContentEditorForm.UpdateEditor(Item folder, Item root, Boolean showEditor) +479
      Sitecore.Shell.Applications.ContentManager.ContentEditorForm.Update() +549
      Sitecore.Shell.Applications.ContentManager.ContentEditorForm.OnPreRendered(EventArgs e) +204

    [TargetInvocationException: Exception has been thrown by the target of an invocation.]
      System.RuntimeMethodHandle.InvokeMethod(Object target, Object[] arguments, Signature sig, Boolean constructor) +0
      System.Reflection.RuntimeMethodInfo.UnsafeInvokeInternal(Object obj, Object[] parameters, Object[] arguments) +128
      System.Reflection.RuntimeMethodInfo.Invoke(Object obj, BindingFlags invokeAttr, Binder binder, Object[] parameters, CultureInfo culture) +142
      Sitecore.Reflection.ReflectionUtil.InvokeMethod(MethodInfo method, Object[] parameters, Object obj) +89
      Sitecore.Shell.Applications.ContentManager.ContentEditorPage.OnPreRender(EventArgs e) +143
      System.Web.UI.Control.PreRenderRecursiveInternal() +200
      System.Web.UI.Page.ProcessRequestMain(Boolean includeStagesBeforeAsyncPoint, Boolean includeStagesAfterAsyncPoint) +7479
      ```

7. I decompiled the `Sitecore.Commerce.XA.Foundation.Upgrade.Pipelines.ContentVersionWarning` and `Sitecore.Commerce.XA.Foundation.Upgrade.Services.UpgradeService` classes to find that it is looking for specific items under `/sitecore/system/Settings/Foundation/Experience Accelerator/Upgrade` in the context database.
8. With the help of the Sitecore DB Browser admin page, I validated these items were not published to the web database, but there were a lot of items in the same situation.
9. I switched back to the master database and did a publish item operation with the "Publish subitems" option checked on `/sitecore/system/Settings`.
10. I was then able to open the Content Editor for the web database, but the published website was still not working.
11. The published website started working only when I did a publish item operation with the "Publish subitems" option checked on `/sitecore`.

## Conclusion

I think the Sitecore Publishing Service has trouble comparing databases content after an aborted classic Sitecore publishing operation. Doing a publish item operation with the "Publish subitems" option checked on `/sitecore` fixes the issue.