---
title: Corrupted Sitecore Instance Manager Export When WFFM Is Installed
tags: [Sitecore, SIM Tool, WFFM]
---

Yesterday, I was troubleshooting a corrupted SIM Tool instance export of Habitat. The exported ZIP file could not be imported back and 7-Zip identified CRC errors in 2 files.

<!-- more -->

* /Website/Data/Sitecore_Wffm.ldf
* /Website/Data/Sitecore_Wffm.mdf

The Web Forms for Marketers (WFFM) installation guide do not mention those files. I found out they are used to switch the storage location of form data from the xDB to an SQL database (see [Use a custom SQL provider to store form data](https://doc.sitecore.net/web_forms_for_marketers/setting_up_web_forms/installing/use_a_custom_sql_provider_to_store_form_data)).

## Solution

Since I did not need those files, I simply deleted them from my web root. The next SIM Tool export was successful and valid.