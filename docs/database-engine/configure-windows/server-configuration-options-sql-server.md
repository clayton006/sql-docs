---
title: Server Configuration Options (SQL Server)
description: Find out how to manage and optimize SQL Server resources. View available configuration options, possible settings, default values, and restart requirements.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
keywords: 
  - "server configuration (SQL Server)"
helpviewer_keywords: 
  - "surface area configuration [SQL Server], sp_configure"
  - "configuration options [SQL Server], when take effect"
  - "server management [SQL Server], configuration options"
  - "SQL Server Management Studio [SQL Server], servers"
  - "servers [SQL Server], configuring"
  - "configuration options [SQL Server], setting"
  - "options [SQL Server], configuration"
  - "RECONFIGURE statement"
  - "performance [SQL Server], servers"
  - "configuration options [SQL Server]"
  - "RECONFIGURE WITH OVERRIDE statement"
  - "SQL Server, configuring"
  - "sp_configure"
  - "stored procedures [SQL Server], configuration options"
  - "server configuration [SQL Server]"
  - "administering SQL Server, configuration options"
author: rwestMSFT
ms.author: randolphwest
ms.reviewer: mikeray
ms.custom: ""
ms.date: 05/14/2021
---

# Server Configuration Options (SQL Server)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

You can manage and optimize SQL Server resources through configuration options by using [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] or the sp_configure system stored procedure. The most commonly used server configuration options are available through [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]; all configuration options are accessible through sp_configure. Consider the effects on your system carefully before setting these options. For more information, see [View or Change Server Properties &#40;SQL Server&#41;](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

> [!IMPORTANT]
> **Advanced options should be changed only by an experienced database administrator or certified SQL Server technician.**

## Categories of Configuration Options

Configuration options take effect either:

- Immediately after setting the option and issuing the **RECONFIGURE** (or in some cases, **RECONFIGURE WITH OVERRIDE**) statement. Reconfiguring certain options will invalidate plans in the plan cache, causing new plans to be compiled. For more information, see [DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md).

  \- or -

- After performing the above actions and restarting the instance of SQL Server.

Options that require SQL Server to restart will initially show the changed value only in the value column. After restart, the new value will appear in both the value column and the value_in_use column.

Some options require a server restart before the new configuration value takes effect. If you set the new value and run sp_configure before restarting the server, the new value appears in the configuration options **value** column, but not in the **value_in_use** column. After restarting the server, the new value appears in the **value_in_use** column.

Self-configuring options are those that SQL Server adjusts according to the needs of the system. In most cases, this eliminates the need for setting the values manually. Examples include the **max worker threads** option and the user connections option.

## Configuration Options Table

The following table lists all available configuration options, the range of possible settings, and default values. Configuration options are marked with letter codes as follows:

- A = Advanced options, which should be changed only by an experienced database administrator or a certified SQL Server professional, and which require setting show advanced options to 1.

- RR = Options requiring a restart of the [!INCLUDE[ssDE](../../includes/ssde-md.md)].

- RP = Options that require a restart of the PolyBase Engine.

- SC = Self-configuring options.

| Configuration option | Minimum value | Maximum value | Default |
|--|--|--|--|
| [access check cache bucket count](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md) (A) | 0 | 16384 | 0 |
| [access check cache quota](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md) (A) | 0 | 2147483647 | 0 |
| [ad hoc distributed queries](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [ADR cleaner retry timeout (min)](../../database-engine/configure-windows/adr-cleaner-retry-timeout-configuration-option.md)<br><br> Introduced in SQL Server 2019 | 0 | 32767 | 15 |
| [ADR Preallocation Factor](../../database-engine/configure-windows/adr-preallocation-factor-server-configuration-option.md)<br><br> Introduced in SQL Server 2019 | 0 | 32767 | 4 |
| [affinity I/O mask](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md) (A, RR) | -2147483648 | 2147483647 | 0 |
| [affinity mask](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md) (A) | -2147483648 | 2147483647 | 0 |
| [affinity64 I/O mask](../../database-engine/configure-windows/affinity64-input-output-mask-server-configuration-option.md) (A, only available on 64-bit version of SQL Server) | -2147483648 | 2147483647 | 0 |
| [affinity64 mask](../../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) (A, RR), only available on 64-bit version of SQL Server | -2147483648 | 2147483647 | 0 |
| [Agent XPs](../../database-engine/configure-windows/agent-xps-server-configuration-option.md) (A) | 0 | 1 | 0<br /><br /> (Changes to 1 when SQL Server Agent is started. Default value is 0 if SQL Server Agent is set to automatic start during Setup.) |
| [allow polybase export](../../database-engine/configure-windows/allow-polybase-export.md)<br/><br/> [!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)].| 0 | 1 | 0 |
| [allow updates](../../database-engine/configure-windows/allow-updates-server-configuration-option.md) (Obsolete. Don't use. Will cause an error during reconfigure.) | 0 | 1 | 0 |
| [automatic soft-NUMA disabled](soft-numa-sql-server.md) | 0 | 1 | 0 |
| [backup checksum default](../../database-engine/configure-windows/backup-checksum-default.md) | 0 | 1 | 0 |
| [backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) | 0 | 1 | 0 |
| [blocked process threshold](../../database-engine/configure-windows/blocked-process-threshold-server-configuration-option.md) (A) | 5 | 86400 | 0 |
| [c2 audit mode](../../database-engine/configure-windows/c2-audit-mode-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) | 0 | 1 | 0 |
| [clr strict security](../../database-engine/configure-windows/clr-strict-security.md) (A) <br /> [!INCLUDE [sqlserver2017](../../includes/applies-to-version/sqlserver2017.md)]. | 0 | 1 | 0 |
| [column encryption enclave type ](../../database-engine/configure-windows/configure-column-encryption-enclave-type.md) (A, RR) | 0 | 1 | 0 |
| [common criteria compliance enabled](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [contained database authentication](../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md) | 0 | 1 | 0 |
| [cost threshold for parallelism](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md) (A) | 0 | 32767 | 5 |
| [cross db ownership chaining](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md) | 0 | 1 | 0 |
| [cursor threshold](../../database-engine/configure-windows/configure-the-cursor-threshold-server-configuration-option.md) (A) | -1 | 2147483647 | -1 |
| [Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [default full-text language](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md) (A) | 0 | 2147483647 | 1033 |
| [default language](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) | 0 | 9999 | 0 |
| [default trace enabled](../../database-engine/configure-windows/default-trace-enabled-server-configuration-option.md) (A) | 0 | 1 | 1 |
| [disallow results from triggers](../../database-engine/configure-windows/disallow-results-from-triggers-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [EKM provider enabled](../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md) | 0 | 1 | 0 |
| [external scripts enabled](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md) (SC) (RR)<br /><br />[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]. | 0 | 1 | 0 |
| [filestream access level](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md) | 0 | 2 | 0 |
| [fill factor](../../database-engine/configure-windows/configure-the-fill-factor-server-configuration-option.md) (A, RR) | 0 | 100 | 0 |
| [ft crawl bandwidth (max)](../../database-engine/configure-windows/ft-crawl-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 100 |
| [ft crawl bandwidth (min)](../../database-engine/configure-windows/ft-crawl-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 0 |
| [ft notify bandwidth (max)](../../database-engine/configure-windows/ft-notify-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 100 |
| [ft notify bandwidth (min)](../../database-engine/configure-windows/ft-notify-bandwidth-server-configuration-option.md)(A) | 0 | 32767 | 0 |
| [hadoop connectivity](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) (RP)<br /><br />[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]. | 0 | 7 | 0 |
| [in-doubt xact resolution](../../database-engine/configure-windows/in-doubt-xact-resolution-server-configuration-option.md) (A) | 0 | 2 | 0 |
| [index create memory](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md) (A, SC) | 704 | 2147483647 | 0 |
| [lightweight pooling](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [locks](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md) (A, RR, SC) | 5000 | 2147483647 | 0 |
| [max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) (A) | 0 | 32767 | 0 |
| [max full-text crawl range](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md) (A) | 0 | 256 | 4 |
| [max server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md) (A, SC) | 16 | 2147483647 | 2147483647 |
| [max text repl size](../../database-engine/configure-windows/configure-the-max-text-repl-size-server-configuration-option.md) | 0 | 2147483647 | 65536 |
| [max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) (A) | 128 | 32767<br /><br /> 1024 is the maximum recommended for 32-bit SQL Server, and 2048 for 64-bit SQL Server. **Note:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] was the last version available on 32-bit operating system. | 0<br /><br /> Zero auto-configures the number of max worker threads depending on the number of processors, using the formula (256 + (*\<processors>* -4) * 8) for 32-bit SQL Server and (512 + (*\<processors>* - 4) * 8) for 64-bit SQL Server. **Note:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] was the last version available on 32-bit operating system. |
| [media retention](../../database-engine/configure-windows/configure-the-media-retention-server-configuration-option.md) (A, RR) | 0 | 365 | 0 |
| [min memory per query](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md) (A) | 512 | 2147483647 | 1024 |
| [min server memory](../../database-engine/configure-windows/server-memory-server-configuration-options.md) (A, SC) | 0 | 2147483647 | 0 |
| [nested triggers](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md) | 0 | 1 | 1 |
| [network packet size](../../database-engine/configure-windows/configure-the-network-packet-size-server-configuration-option.md) (A) | 512 | 32767 | 4096 |
| [Ole Automation Procedures](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [open objects](../../database-engine/configure-windows/open-objects-server-configuration-option.md) (A, RR, obsolete) | 0 | 2147483647 | 0 |
| [optimize for ad hoc workloads](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [PH_timeout](../../database-engine/configure-windows/ph-timeout-server-configuration-option.md) (A) | 1 | 3600 | 60 |
| [polybase enabled](../../relational-databases/polybase/polybase-installation.md#enable) (RR) <br/><br/>[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]| 0 | 1 | 0 |
| [polybase network encryption](../../relational-databases/polybase/polybase-installation.md#enable) | 0 | 1 | 1 |
| [precompute rank](../discontinued-database-engine-functionality-in-sql-server.md) (A) | 0 | 1 | 0 |
| [priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [query governor cost limit](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md) (A) | 0 | 2147483647 | 0 |
| [query wait](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md) (A) | -1 | 2147483647 | -1 |
| [recovery interval](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md) (A, SC) | 0 | 32767 | 0 |
| [remote access](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md) (RR) | 0 | 1 | 1 |
| [remote admin connections](../../database-engine/configure-windows/remote-admin-connections-server-configuration-option.md) | 0 | 1 | 0 |
| [remote data archive](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md) | 0 | 1 | 0 |
| [remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md) | 0 | 2147483647 | 10 |
| [remote proc trans](../../database-engine/configure-windows/configure-the-remote-proc-trans-server-configuration-option.md) | 0 | 1 | 0 |
| [remote query timeout](../../database-engine/configure-windows/configure-the-remote-query-timeout-server-configuration-option.md) | 0 | 2147483647 | 600 |
| [Replication XPs Option](../../database-engine/configure-windows/replication-xps-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [scan for startup procs](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md) (A, RR) | 0 | 1 | 0 |
| [server trigger recursion](../../database-engine/configure-windows/server-trigger-recursion-server-configuration-option.md) | 0 | 1 | 1 |
| [set working set size](../../database-engine/configure-windows/set-working-set-size-server-configuration-option.md) (A, RR, obsolete) | 0 | 1 | 0 |
| [show advanced options](../../database-engine/configure-windows/show-advanced-options-server-configuration-option.md) | 0 | 1 | 0 |
| [SMO and DMO XPs](../../database-engine/configure-windows/smo-and-dmo-xps-server-configuration-option.md) (A) | 0 | 1 | 1 |
| [suppress recovery model errors](../../database-engine/configure-windows/suppress-recovery-model-errors-server-configuration-option.md) (A) <br/><br/>[!INCLUDE [asdbmi](../../includes/applies-to-version/_asmi.md)]| 0 | 1 | 0 |
| [tempdb metadata memory-optimized](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) (A) <br/><br/> [!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)].| 0 | 1 | 0 |
| [transform noise words](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md) (A) | 0 | 1 | 0 |
| [two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) (A) | 1753 | 9999 | 2049 |
| [user connections](../../database-engine/configure-windows/configure-the-user-connections-server-configuration-option.md) (A, RR, SC) | 0 | 32767 | 0 |
| [user options](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md) | 0 | 32767 | 0 |
| [xp_cmdshell](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md) (A) | 0 | 1 | 0 |

## See also

- [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 
- [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)
- [DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)