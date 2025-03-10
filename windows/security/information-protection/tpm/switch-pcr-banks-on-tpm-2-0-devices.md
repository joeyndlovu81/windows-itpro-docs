---
title: Understanding PCR banks on TPM 2.0 devices (Windows)
description: This topic for the IT professional provides background about what happens when you switch PCR banks on TPM 2.0 devices.
ms.reviewer: 
ms.prod: windows-client
author: dansimp
ms.author: dansimp
manager: aaroncz
ms.collection: 
  - M365-security-compliance
ms.topic: conceptual
ms.date: 09/06/2021
---

# Understanding PCR banks on TPM 2.0 devices

**Applies to**
-   Windows 10
-   Windows 11
-   Windows Server 2016 and above

For steps on how to switch PCR banks on TPM 2.0 devices on your PC, you should contact your OEM or UEFI vendor. This topic provides background about what happens when you switch PCR banks on TPM 2.0 devices.

A Platform Configuration Register (PCR) is a memory location in the TPM that has some unique properties. The size of the value that can be stored in a PCR is determined by the size of a digest generated by an associated hashing algorithm. A SHA-1 PCR can store 20 bytes – the size of a SHA-1 digest. Multiple PCRs associated with the same hashing algorithm are referred to as a PCR bank.

To store a new value in a PCR, the existing value is extended with a new value as follows:
PCR\[N\] = HASHalg( PCR\[N\] || ArgumentOfExtend )

The existing value is concatenated with the argument of the TPM Extend operation. The resulting concatenation is then used as input to the associated hashing algorithm, which computes a digest of the input. This computed digest becomes the new value of the PCR.

The [TCG PC Client Platform TPM Profile Specification](http://www.trustedcomputinggroup.org/pc-client-platform-tpm-profile-ptp-specification/) defines the inclusion of at least one PCR bank with 24 registers. The only way to reset the first 16 PCRs is to reset the TPM itself. This restriction helps ensure that the value of those PCRs can only be modified via the TPM Extend operation.

Some TPM PCRs are used as checksums of log events. The log events are extended in the TPM as the events occur. Later, an auditor can validate the logs by computing the expected PCR values from the log and comparing them to the PCR values of the TPM. Since the first 16 TPM PCRs cannot be modified arbitrarily, a match between an expected PCR value in that range and the actual TPM PCR value provides assurance of an unmodified log.

## How does Windows use PCRs?

To bind the use of a TPM based key to a certain state of the PC, the key can be sealed to an expected set of PCR values. For instance, PCRs 0 through 7 have a well-defined value after the boot process – when the OS is loaded. When the hardware, firmware, or boot loader of the machine changes, the change can be detected in the PCR values. Windows uses this capability to make certain cryptographic keys only available at certain times during the boot process. For instance, the BitLocker key can be used at a certain point in the boot, but not before or after.

It is important to note that this binding to PCR values also includes the hashing algorithm used for the PCR. For instance, a key can be bound to a specific value of the SHA-1 PCR\[12\], if using SHA-256 PCR banks, even with the same system configuration. Otherwise, the PCR values will not match.

## What happens when PCR banks are switched?

When the PCR banks are switched, the algorithm used to compute the hashed values stored in the PCRs during extend operations is changed. Each hash algorithm will return a different cryptographic signature for the same inputs.

As a result, if the currently used PCR bank is switched all keys that have been bound to the previous PCR values will no longer work. For example, if you had a key bound to the SHA-1 value of PCR\[12\] and subsequently changed the PCR banks to SHA-256, the banks wouldn’t match, and you would be unable to use that key. The BitLocker key is secured using the PCR banks and Windows will not be able to unseal it if the PCR banks are switched while BitLocker is enabled.

## What can I do to switch PCRs when BitLocker is already active?

Before switching PCR banks you should suspend or disable BitLocker – or have your recovery key ready. For steps on how to switch PCR banks on your PC, you should contact your OEM or UEFI vendor.

## How can I identify which PCR bank is being used?

A TPM can be configured to have multiple PCR banks active. When BIOS is performing measurements it will do so into all active PCR banks, depending on its capability to make these measurements. BIOS may chose to deactivate PCR banks that it does not support or "cap" PCR banks that it does not support by extending a separator. The following registry value identifies which PCR banks are active.

- Registry key: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\IntegrityServices<br>
- DWORD: TPMActivePCRBanks<br>
- Defines which PCR banks are currently active. (This value should be interpreted as a bitmap for which the bits are defined in the [TCG Algorithm Registry](https://trustedcomputinggroup.org/resource/tcg-algorithm-registry/) Table 21 of Revision 1.27.)<br>

Windows checks which PCR banks are active and supported by the BIOS. Windows also checks if the measured boot log supports measurements for all active PCR banks. Windows will prefer the use of the SHA-256 bank for measurements and will fall back to SHA1 PCR bank if one of the pre-conditions is not met.

You can identify which PCR bank is currently used by Windows by looking at the registry.

- Registry key: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\IntegrityServices<br>
- DWORD: TPMDigestAlgID<br>
- Algorithm ID of the PCR bank that Windows is currently using. (This value represents an algorithm identifier as defined in the [TCG Algorithm Registry](https://trustedcomputinggroup.org/resource/tcg-algorithm-registry/) Table 3 of Revision 1.27.)<br>

Windows only uses one PCR bank to continue boot measurements. All other active PCR banks will be extended with a separator to indicate that they are not used by Windows and measurements that appear to be from Windows should not be trusted.

## Related topics

- [Trusted Platform Module](trusted-platform-module-top-node.md) (list of topics)
