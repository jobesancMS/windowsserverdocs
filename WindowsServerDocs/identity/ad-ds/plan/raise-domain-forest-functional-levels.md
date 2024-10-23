---
title: Raise domain and forest functional levels in Active Directory Domain Services on Windows Server
description: Learn how to raise Active Directory domain and forest functional levels on Windows Server
ms.topic: how-to
ms.author: roharwoo
author: gswashington
ms.date: 09/26/2024
---
# Raise domain and forest functional levels in Active Directory Domain Services

>Applies to: Windows Server 2025 (preview), Windows Server 2022, Windows Server 2019, Windows Server 2016

> [!IMPORTANT]
> Windows Server 2025 is in PREVIEW. This information relates to a prerelease product that may be substantially modified before it's released. Microsoft makes no warranties, expressed or implied, with respect to the information provided here.

Functional levels determine the available Active Directory Domain Services (AD DS) domain or forest capabilities. Functional levels also determine which Windows Server operating systems you can run on domain controllers in the domain or forest. Level changes happen when you use later versions of your domain controller operating system, the domain, or your forest functional level. This article describes how to raise Active Directory domain and forest functional levels. We recommend you upgrade Active Directory Domain Service servers to the latest release.

To enable the latest domain features, all domain controllers in the domain must run the version of Windows Server that matches or is newer than the desired functional level. If they don't meet this requirement, the administrator can't raise the domain functional level.

To enable the latest forest-wide features, all domain controllers in the forest must run the Windows Server operating system version that matches or is newer than the desired functional level. The current domain functional level must already be at the latest level. If the forest meets these requirements, the administrator can raise the forest functional level.

The domain and forest functional levels only affect how the domain controllers operate together as a group. The clients that interact with the domain or with the forest are unaffected by the changes. Applications are also unaffected by these changes. However, applications can use new features found in later versions of Windows Server once the administrator raises the domain level. For more information about the functional levels, see [Active Directory Domain Services functional levels](/windows-server/identity/ad-ds/active-directory-functional-levels).

> [!WARNING]
> Changes to the domain and forest functional levels are irreversible. In order to undo the change, you must perform a forest recovery to revert to an earlier point in time.

## Prerequisites

You need to complete the following things to raise the domain functional level:

- All domain controllers in the domain are running at least the version of Windows Server that you want to raise the domain functional level to. For example, to raise the domain functional level to Windows Server 2025, all domain controllers in the domain must be running Windows Server 2025. If you have domain controllers running earlier versions of Windows Server, you must upgrade them to Windows Server 2025 before you can raise the domain functional level.

- Before you can promote a machine running Windows Server 2025 to a domain controller in an existing domain, that domain must also be at least at the Windows Server 2016 functional level. Earlier versions of Windows Server don't support Windows Server 2025 domain controllers.

- Your Active Directory forest and domain is operational and free from replication errors. To learn more about replication errors, see [Diagnose Active Directory replication failures](/troubleshoot/windows-server/active-directory/diagnose-replication-failures).

- Identify all your DCs hosting the Global Catalog (GC) and FSMO roles. Create and verify backups of these domain controllers before making changes.

- You must be a member of the Enterprise Admins group or equivalent to raise the forest functional level.

- You must have a computer with either of the following Remote Server Administration Tools (RSAT) installed:

  - AD DS Tools.

    OR

  - Active Directory module for Windows PowerShell.

## View the current functional level

To view the current domain and forest functional levels, you can use the Active Directory Domains and Trusts console or Windows PowerShell. The following sections describe how to raise the functional levels using these methods.

> [!NOTE]
> Windows Server 2019 and Windows Server 2022 use Windows Server 2016 as the most recent functional levels.

### [Desktop](#tab/desktop)

To raise the domain or forest functional level using the Active Directory Domains and Trusts console, follow these steps.

1. Sign in to a computer with the AD DS Remote Server Administration Tools (RSAT) installed.

1. Select the Start menu, then enter _Active Directory Domains and Trusts_ in the search box.

1. Open the **Active Directory Domains and Trusts** app.

1. In the console tree, right-click the domain node, and then select **Properties**.

1. The **Properties** dialog box shows the current domain and forest functional level.

### [PowerShell](#tab/PowerShell)

To view the domain or forest functional level using PowerShell, follow these steps.

1. Sign in to a computer with the AD DS Remote Server Administration Tools (RSAT) installed.

1. Open PowerShell as an administrator.

1. Run the following command to view the current domain functional level, replacing `<domain>` with the domain name.

   ```powershell
   Get-ADDomain -Identity <domain> | Select-Object DomainMode
   ```

1. Run the following command to view the current forest functional level, replacing `<forest>` with the forest name.

   ```powershell
   Get-ADForest -Identity <forest> | Select-Object ForestMode
   ```

For more information about the `Get-ADDomain` and `Get-ADForest` cmdlets, see [Get-ADDomain](/powershell/module/activedirectory/get-addomain) and [Get-ADForest](/powershell/module/activedirectory/get-adforest).

---

## Raise the functional level

To raise the domain and forest functional levels, you can use the Active Directory Domains and Trusts console or Windows PowerShell. The following sections describe how to raise the functional levels using these methods.

> [!WARNING]
> Changes to the domain and forest functional levels are irreversible. In order to undo the change, you must perform a forest recovery to revert to an earlier point of time.

### [Desktop](#tab/desktop)

To raise the domain or forest functional level using the Active Directory Domains and Trusts console, follow these steps.

1. Sign in to a computer with the AD DS Remote Server Administration Tools (RSAT) installed.

1. Select the Start menu, then enter _Active Directory Domains and Trusts_ in the search box.

1. Open the **Active Directory Domains and Trusts** app.

1. In the console tree, right-click the domain node, and then select **Raise Domain Functional Level**.

1. In **Select an available domain functional level**, select the value and then select **Raise**.

1. When prompted, select **OK** to confirm the change.

1. Once the domain functional level is raised, raise the forest functional level. In the console tree, right-click **Active Directory Domains and Trusts** , and then select **Raise Forest Functional Level**.

1. In **Select an available forest functional level**, select the value and then select **Raise**.

You've now raised the domain and forest functional level.

### [PowerShell](#tab/PowerShell)

To raise the domain or forest functional level using PowerShell, follow these steps.

1. Sign in to a computer with the AD DS Remote Server Administration Tools (RSAT) installed.

1. Open PowerShell as an administrator.

1. Run the following command to raise the domain functional level, replacing `<domain>` with the domain name and `<level>` with the desired domain functional level.
  
      ```powershell
      Set-ADDomainMode -Identity <domain> -DomainMode <level>
      ```

1. To confirm the change, select **Y**.

1. Once the domain functional level is raised, run the following command to raise the forest functional level, replacing `<level>` with the desired forest functional level.

      ```powershell
      Set-ADForestMode -Identity <forest> -ForestMode <level>
      ```

1. To confirm the change, select **Y**.

You've now raised the domain and forest functional level. For more information about the `Set-ADDomainMode` and `Set-ADForestMode` cmdlets, see [Set-ADDomainMode](/powershell/module/activedirectory/set-addomainmode) and [Set-ADForestMode](/powershell/module/activedirectory/set-adforestmode).

---

## Related content

- [Forest and Domain Functional Levels](../active-directory-functional-levels.md)

- [Identifying Your Functional Level Upgrade](Identifying-Your-Functional-Level-Upgrade.md)