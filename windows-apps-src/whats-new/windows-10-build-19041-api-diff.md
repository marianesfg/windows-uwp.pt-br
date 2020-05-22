---
title: Alterações na API do Windows 10 build 19041
description: Os desenvolvedores podem usar a lista a seguir para identificar namespaces novos ou alterados no Windows 10 build 19041
keywords: Windows 10, apis, 19041, 2004
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9158fe5bdbd93e60bb6eed4476bb29b3ad07c4b6
ms.sourcegitcommit: 0f2ae8f97daac440c8e86dc07d11d356de29515c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83342868"
---
# <a name="new-apis-in-windows-10-build-19041"></a>Novas APIs no Windows 10 build 19041

Os namespaces de API novos e atualizados foram disponibilizados para desenvolvedores no Windows 10 build 19041 (também conhecido como SDK versão 2004). Veja abaixo uma lista completa de documentação publicada para namespaces adicionados ou modificados nesta versão.

Para saber mais sobre as API adicionadas na versão pública anterior, confira [Novas APIs na Atualização de outubro do Windows 10](windows-10-build-18362-api-diff.md).

## <a name="windowsai"></a>Windows.AI

### <a name="windowsaimachinelearning"></a>[Windows.AI.MachineLearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning)

#### <a name="learningmodelsessionoptions"></a>[LearningModelSessionOptions](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions)

LearningModelSessionOptions.CloseModelOnSessionCreation

## <a name="windowsapplicationmodel"></a>Windows.ApplicationModel

### <a name="windowsapplicationmodelbackground"></a>[Windows.ApplicationModel.Background](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background)

#### <a name="backgroundtaskbuilder"></a>[BackgroundTaskBuilder](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder)

BackgroundTaskBuilder.SetTaskEntryPointClsid

#### <a name="bluetoothleadvertisementpublishertrigger"></a>[BluetoothLEAdvertisementPublisherTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.bluetoothleadvertisementpublishertrigger)

BluetoothLEAdvertisementPublisherTrigger.IncludeTransmitPowerLevel <br> BluetoothLEAdvertisementPublisherTrigger.IsAnonymous <br> BluetoothLEAdvertisementPublisherTrigger.PreferredTransmitPowerLevelInDBm <br> BluetoothLEAdvertisementPublisherTrigger.UseExtendedFormat

#### <a name="bluetoothleadvertisementwatchertrigger"></a>[BluetoothLEAdvertisementWatcherTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.bluetoothleadvertisementwatchertrigger)

BluetoothLEAdvertisementWatcherTrigger.AllowExtendedAdvertisements

### <a name="windowsapplicationmodelconversationalagent"></a>[Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent)

#### <a name="activationsignaldetectionconfiguration"></a>[ActivationSignalDetectionConfiguration](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetectionconfiguration)

ActivationSignalDetectionConfiguration <br> ActivationSignalDetectionConfiguration.ApplyTrainingData <br> ActivationSignalDetectionConfiguration.ApplyTrainingDataAsync <br> ActivationSignalDetectionConfiguration.AvailabilityChanged <br> ActivationSignalDetectionConfiguration.AvailabilityInfo <br> ActivationSignalDetectionConfiguration.ClearModelData <br> ActivationSignalDetectionConfiguration.ClearModelDataAsync <br> ActivationSignalDetectionConfiguration.ClearTrainingData <br> ActivationSignalDetectionConfiguration.ClearTrainingDataAsync <br> ActivationSignalDetectionConfiguration.DisplayName <br> ActivationSignalDetectionConfiguration.GetModelData <br> ActivationSignalDetectionConfiguration.GetModelDataAsync <br> ActivationSignalDetectionConfiguration.GetModelDataType <br> ActivationSignalDetectionConfiguration.GetModelDataTypeAsync <br> ActivationSignalDetectionConfiguration.IsActive <br> ActivationSignalDetectionConfiguration.ModelId <br> ActivationSignalDetectionConfiguration.SetEnabled <br> ActivationSignalDetectionConfiguration.SetEnabledAsync <br> ActivationSignalDetectionConfiguration.SetModelData <br> ActivationSignalDetectionConfiguration.SetModelDataAsync <br> ActivationSignalDetectionConfiguration.SignalId <br> ActivationSignalDetectionConfiguration.TrainingDataFormat <br> ActivationSignalDetectionConfiguration.TrainingStepsCompleted <br> ActivationSignalDetectionConfiguration.TrainingStepsRemaining

#### <a name="activationsignaldetectiontrainingdataformat"></a>[ActivationSignalDetectionTrainingDataFormat](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetectiontrainingdataformat)

ActivationSignalDetectionTrainingDataFormat

#### <a name="activationsignaldetector"></a>[ActivationSignalDetector](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetector)

ActivationSignalDetector <br> ActivationSignalDetector.CanCreateConfigurations <br> ActivationSignalDetector.CreateConfiguration <br> ActivationSignalDetector.CreateConfigurationAsync <br> ActivationSignalDetector.GetConfiguration <br> ActivationSignalDetector.GetConfigurationAsync <br> ActivationSignalDetector.GetConfigurations <br> ActivationSignalDetector.GetConfigurationsAsync <br> ActivationSignalDetector.GetSupportedModelIdsForSignalId <br> ActivationSignalDetector.GetSupportedModelIdsForSignalIdAsync <br> ActivationSignalDetector.Kind <br> ActivationSignalDetector.ProviderId <br> ActivationSignalDetector.RemoveConfiguration <br> ActivationSignalDetector.RemoveConfigurationAsync <br> ActivationSignalDetector.SupportedModelDataTypes <br> ActivationSignalDetector.SupportedPowerStates <br> ActivationSignalDetector.SupportedTrainingDataFormats

#### <a name="activationsignaldetectorkind"></a>[ActivationSignalDetectorKind](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetectorkind)

ActivationSignalDetectorKind

#### <a name="activationsignaldetectorpowerstate"></a>[ActivationSignalDetectorPowerState](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetectorpowerstate)

ActivationSignalDetectorPowerState

#### <a name="conversationalagentdetectormanager"></a>[ConversationalAgentDetectorManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentdetectormanager)

ConversationalAgentDetectorManager <br> ConversationalAgentDetectorManager.Default <br> ConversationalAgentDetectorManager.GetActivationSignalDetectors <br> ConversationalAgentDetectorManager.GetActivationSignalDetectorsAsync <br> ConversationalAgentDetectorManager.GetAllActivationSignalDetectors <br> ConversationalAgentDetectorManager.GetAllActivationSignalDetectorsAsync

#### <a name="detectionconfigurationavailabilitychangekind"></a>[DetectionConfigurationAvailabilityChangeKind](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.detectionconfigurationavailabilitychangedeventargs)

DetectionConfigurationAvailabilityChangeKind <br> DetectionConfigurationAvailabilityChangedEventArgs

#### <a name="detectionconfigurationavailabilitychangedeventargs"></a>[DetectionConfigurationAvailabilityChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.detectionconfigurationavailabilitychangekind)

DetectionConfigurationAvailabilityChangedEventArgs.Kind

#### <a name="detectionconfigurationavailabilityinfo"></a>[DetectionConfigurationAvailabilityInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.detectionconfigurationavailabilityinfo)

DetectionConfigurationAvailabilityInfo <br> DetectionConfigurationAvailabilityInfo.HasLockScreenPermission <br> DetectionConfigurationAvailabilityInfo.HasPermission <br> DetectionConfigurationAvailabilityInfo.HasSystemResourceAccess <br> DetectionConfigurationAvailabilityInfo.IsEnabled

#### <a name="detectionconfigurationtrainingstatus"></a>[DetectionConfigurationTrainingStatus](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.detectionconfigurationtrainingstatus)

DetectionConfigurationTrainingStatus

### <a name="windowsapplicationmodel"></a>[Windows.ApplicationModel](https://docs.microsoft.com/uwp/api/windows.applicationmodel)

#### <a name="appinfo"></a>[AppInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinfo)

AppInfo.Current <br> AppInfo.GetFromAppUserModelId <br> AppInfo.GetFromAppUserModelIdForUser <br> AppInfo.Package

#### <a name="iappinfostatics"></a>[IAppInfoStatics](https://docs.microsoft.com/uwp/api/windows.applicationmodel.iappinfostatics)

IAppInfoStatics <br> IAppInfoStatics.Current <br> IAppInfoStatics.GetFromAppUserModelId <br> IAppInfoStatics.GetFromAppUserModelIdForUser

#### <a name="package"></a>[Package](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package)

Package.EffectiveExternalLocation <br> Package.EffectiveExternalPath <br> Package.EffectivePath <br> Package.GetAppListEntries <br> Package.GetLogoAsRandomAccessStreamReference <br> Package.InstalledPath <br> Package.IsStub <br> Package.MachineExternalLocation <br> Package.MachineExternalPath <br> Package.MutablePath <br> Package.UserExternalLocation <br> Package.UserExternalPath

## <a name="windowsdevices"></a>Windows.Devices

### <a name="windowsdevicesbluetoothadvertisement"></a>[Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)

#### <a name="bluetoothleadvertisementpublisher"></a>[BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher)

BluetoothLEAdvertisementPublisher.IncludeTransmitPowerLevel <br> BluetoothLEAdvertisementPublisher.IsAnonymous <br> BluetoothLEAdvertisementPublisher.PreferredTransmitPowerLevelInDBm <br> BluetoothLEAdvertisementPublisher.UseExtendedAdvertisement

#### <a name="bluetoothleadvertisementpublisherstatuschangedeventargs"></a>[BluetoothLEAdvertisementPublisherStatusChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisherstatuschangedeventargs)

BluetoothLEAdvertisementPublisherStatusChangedEventArgs.SelectedTransmitPowerLevelInDBm

#### <a name="bluetoothleadvertisementreceivedeventargs"></a>[BluetoothLEAdvertisementReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementreceivedeventargs)

BluetoothLEAdvertisementReceivedEventArgs.BluetoothAddressType <br> BluetoothLEAdvertisementReceivedEventArgs.IsAnonymous <br> BluetoothLEAdvertisementReceivedEventArgs.IsAnonymous <br> BluetoothLEAdvertisementReceivedEventArgs.IsDirected <br> BluetoothLEAdvertisementReceivedEventArgs.IsScanResponse <br> BluetoothLEAdvertisementReceivedEventArgs.IsScannable <br> BluetoothLEAdvertisementReceivedEventArgs.TransmitPowerLevelInDBm

#### <a name="bluetoothleadvertisementwatcher"></a>[BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher)

BluetoothLEAdvertisementWatcher.AllowExtendedAdvertisements

### <a name="windowsdevicesbluetoothbackground"></a>[Windows.Devices.Bluetooth.Background](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.background)

#### <a name="bluetoothleadvertisementpublishertriggerdetails"></a>[BluetoothLEAdvertisementPublisherTriggerDetails](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.background.bluetoothleadvertisementpublishertriggerdetails)

BluetoothLEAdvertisementPublisherTriggerDetails.SelectedTransmitPowerLevelInDBm

### <a name="windowsdevicesbluetooth"></a>[Windows.Devices.Bluetooth](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth)

#### <a name="bluetoothadapter"></a>[BluetoothAdapter](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothadapter)

BluetoothAdapter.IsExtendedAdvertisingSupported <br> BluetoothAdapter.MaxAdvertisementDataLength

### <a name="windowsdevicesdisplay"></a>[Windows.Devices.Display](https://docs.microsoft.com/uwp/api/windows.devices.display)

#### <a name="displaymonitor"></a>[DisplayMonitor](https://docs.microsoft.com/uwp/api/windows.devices.display.displaymonitor)

DisplayMonitor.IsDolbyVisionSupportedInHdrMode

### <a name="windowsdevicesinput"></a>[Windows.Devices.Input](https://docs.microsoft.com/uwp/api/windows.devices.input)

#### <a name="penbuttonlistener"></a>[PenButtonListener](https://docs.microsoft.com/uwp/api/windows.devices.input.penbuttonlistener)

PenButtonListener <br> PenButtonListener.GetDefault <br> PenButtonListener.IsSupported <br> PenButtonListener.IsSupportedChanged <br> PenButtonListener.TailButtonClicked <br> PenButtonListener.TailButtonDoubleClicked <br> PenButtonListener.TailButtonLongPressed

#### <a name="pendocklistener"></a>[PenDockListener](https://docs.microsoft.com/uwp/api/windows.devices.input.pendockedeventargs)

PenDockListener

#### <a name="pendocklistener"></a>[PenDockListener](https://docs.microsoft.com/uwp/api/windows.devices.input.pendocklistener)

PenDockListener.Docked <br> PenDockListener.GetDefault <br> PenDockListener.IsSupported <br> PenDockListener.IsSupportedChanged <br> PenDockListener.Undocked <br> PenDockedEventArgs

#### <a name="pentailbuttonclickedeventargs"></a>[PenTailButtonClickedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.pentailbuttonclickedeventargs)

PenTailButtonClickedEventArgs

#### <a name="pentailbuttondoubleclickedeventargs"></a>[PenTailButtonDoubleClickedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.pentailbuttondoubleclickedeventargs)

PenTailButtonDoubleClickedEventArgs

#### <a name="pentailbuttonlongpressedeventargs"></a>[PenTailButtonLongPressedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.pentailbuttonlongpressedeventargs)

PenTailButtonLongPressedEventArgs

#### <a name="penundockedeventargs"></a>[PenUndockedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.penundockedeventargs)

PenUndockedEventArgs

### <a name="windowsdevicessensors"></a>[Windows.Devices.Sensors](https://docs.microsoft.com/uwp/api/windows.devices.sensors)

#### <a name="accelerometer"></a>[Accelerometer](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometer)

Accelerometer.ReportThreshold

#### <a name="accelerometerdatathreshold"></a>[AccelerometerDataThreshold](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometerdatathreshold)

AccelerometerDataThreshold <br> AccelerometerDataThreshold.XAxisInGForce <br> AccelerometerDataThreshold.YAxisInGForce <br> AccelerometerDataThreshold.ZAxisInGForce

#### <a name="barometer"></a>[Barometer](https://docs.microsoft.com/uwp/api/windows.devices.sensors.barometer)

Barometer.ReportThreshold

#### <a name="barometerdatathreshold"></a>[BarometerDataThreshold](https://docs.microsoft.com/uwp/api/windows.devices.sensors.barometerdatathreshold)

BarometerDataThreshold <br> BarometerDataThreshold.Hectopascals

#### <a name="compass"></a>[Compass](https://docs.microsoft.com/uwp/api/windows.devices.sensors.compass)

Compass.ReportThreshold

#### <a name="compassdatathreshold"></a>[CompassDataThreshold](https://docs.microsoft.com/uwp/api/windows.devices.sensors.compassdatathreshold)

CompassDataThreshold <br> CompassDataThreshold.Degrees

#### <a name="gyrometer"></a>[Gyrometer](https://docs.microsoft.com/uwp/api/windows.devices.sensors.gyrometer)

Gyrometer.ReportThreshold

#### <a name="gyrometerdatathreshold"></a>[GyrometerDataThreshold](https://docs.microsoft.com/uwp/api/windows.devices.sensors.gyrometerdatathreshold)

GyrometerDataThreshold <br> GyrometerDataThreshold.XAxisInDegreesPerSecond <br> GyrometerDataThreshold.YAxisInDegreesPerSecond <br> GyrometerDataThreshold.ZAxisInDegreesPerSecond

#### <a name="inclinometer"></a>[Inclinometer](https://docs.microsoft.com/uwp/api/windows.devices.sensors.inclinometer)

Inclinometer.ReportThreshold

#### <a name="inclinometerdatathreshold"></a>[InclinometerDataThreshold](https://docs.microsoft.com/uwp/api/windows.devices.sensors.inclinometerdatathreshold)

InclinometerDataThreshold <br> InclinometerDataThreshold.PitchInDegrees <br> InclinometerDataThreshold.RollInDegrees <br> InclinometerDataThreshold.YawInDegrees

#### <a name="lightsensor"></a>[LightSensor](https://docs.microsoft.com/uwp/api/windows.devices.sensors.lightsensor)

LightSensor.ReportThreshold

#### <a name="lightsensordatathreshold"></a>[LightSensorDataThreshold](https://docs.microsoft.com/uwp/api/windows.devices.sensors.lightsensordatathreshold)

LightSensorDataThreshold <br> LightSensorDataThreshold.AbsoluteLux <br> LightSensorDataThreshold.LuxPercentage

#### <a name="magnetometer"></a>[Magnetometer](https://docs.microsoft.com/uwp/api/windows.devices.sensors.magnetometer)

Magnetometer.ReportThreshold

#### <a name="magnetometerdatathreshold"></a>[MagnetometerDataThreshold](https://docs.microsoft.com/uwp/api/windows.devices.sensors.magnetometerdatathreshold)

MagnetometerDataThreshold <br> MagnetometerDataThreshold.XAxisMicroteslas <br> MagnetometerDataThreshold.YAxisMicroteslas <br> MagnetometerDataThreshold.ZAxisMicroteslas

## <a name="windowsfoundation"></a>Windows.Foundation

### <a name="windowsfoundationmetadata"></a>[Windows.Foundation.Metadata](https://docs.microsoft.com/uwp/api/windows.foundation.metadata)

#### <a name="attributenameattribute"></a>[AttributeNameAttribute](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.attributenameattribute)

AttributeNameAttribute <br> AttributeNameAttribute.#ctor

#### <a name="fastabiattribute"></a>[FastAbiAttribute](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.fastabiattribute)

FastAbiAttribute <br> FastAbiAttribute.#ctor <br> FastAbiAttribute.#ctor <br> FastAbiAttribute.#ctor

#### <a name="noexceptionattribute"></a>[NoExceptionAttribute](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.noexceptionattribute)

NoExceptionAttribute <br> NoExceptionAttribute.#ctor

## <a name="windowsglobalization"></a>Windows.Globalization

### <a name="windowsglobalization"></a>[Windows.Globalization](https://docs.microsoft.com/uwp/api/windows.globalization)

#### <a name="language"></a>[Idioma](https://docs.microsoft.com/uwp/api/windows.globalization.language)

Language.AbbreviatedName <br> Language.GetMuiCompatibleLanguageListFromLanguageTags

## <a name="windowsgraphics"></a>Windows.Graphics

### <a name="windowsgraphicscapture"></a>[Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture)

#### <a name="graphicscapturesession"></a>[GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession)

GraphicsCaptureSession.IsCursorCaptureEnabled

### <a name="windowsgraphicsholographic"></a>[Windows.Graphics.Holographic](https://docs.microsoft.com/uwp/api/windows.graphics.holographic)

#### <a name="holographicframe"></a>[HolographicFrame](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicframe)

HolographicFrame.Id

#### <a name="holographicframeid"></a>[HolographicFrameId](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicframeid)

HolographicFrameId

#### <a name="holographicframerenderingreport"></a>[HolographicFrameRenderingReport](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicframerenderingreport)

HolographicFrameRenderingReport <br> HolographicFrameRenderingReport.FrameId <br> HolographicFrameRenderingReport.MissedLatchCount <br> HolographicFrameRenderingReport.SystemRelativeActualGpuFinishTime <br> HolographicFrameRenderingReport.SystemRelativeFrameReadyTime <br> HolographicFrameRenderingReport.SystemRelativeTargetLatchTime

#### <a name="holographicframescanoutmonitor"></a>[HolographicFrameScanoutMonitor](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicframescanoutmonitor)

HolographicFrameScanoutMonitor <br> HolographicFrameScanoutMonitor.Close <br> HolographicFrameScanoutMonitor.ReadReports

#### <a name="holographicframescanoutreport"></a>[HolographicFrameScanoutReport](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicframescanoutreport)

HolographicFrameScanoutReport <br> HolographicFrameScanoutReport.MissedScanoutCount <br> HolographicFrameScanoutReport.RenderingReport <br> HolographicFrameScanoutReport.SystemRelativeLatchTime <br> HolographicFrameScanoutReport.SystemRelativePhotonTime <br> HolographicFrameScanoutReport.SystemRelativeScanoutStartTime

#### <a name="holographicspace"></a>[HolographicSpace](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicspace)

HolographicSpace.CreateFrameScanoutMonitor

## <a name="windowsmanagement"></a>Windows.Management

### <a name="windowsmanagementdeployment"></a>[Windows.Management.Deployment](https://docs.microsoft.com/uwp/api/windows.management.deployment)

#### <a name="addpackageoptions"></a>[AddPackageOptions](https://docs.microsoft.com/uwp/api/windows.management.deployment.addpackageoptions)

AddPackageOptions <br> AddPackageOptions.#ctor <br> AddPackageOptions.AllowUnsigned <br> AddPackageOptions.DeferRegistrationWhenPackagesAreInUse <br> AddPackageOptions.DependencyPackageUris <br> AddPackageOptions.DeveloperMode <br> AddPackageOptions.ExternalLocationUri <br> AddPackageOptions.ForceAppShutdown <br> AddPackageOptions.ForceTargetAppShutdown <br> AddPackageOptions.ForceUpdateFromAnyVersion <br> AddPackageOptions.InstallAllResources <br> AddPackageOptions.OptionalPackageFamilyNames <br> AddPackageOptions.OptionalPackageUris <br> AddPackageOptions.RelatedPackageUris <br> AddPackageOptions.RequiredContentGroupOnly <br> AddPackageOptions.RetainFilesOnFailure <br> AddPackageOptions.StageInPlace <br> AddPackageOptions.StubPackageOption <br> AddPackageOptions.TargetVolume

#### <a name="packagemanager"></a>[PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)

PackageManager.AddPackageByUriAsync <br> PackageManager.FindProvisionedPackages <br> PackageManager.GetPackageStubPreference <br> PackageManager.RegisterPackageByUriAsync <br> PackageManager.RegisterPackagesByFullNameAsync <br> PackageManager.SetPackageStubPreference <br> PackageManager.StagePackageByUriAsync

#### <a name="packagestubpreference"></a>[PackageStubPreference](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagestubpreference)

PackageStubPreference

#### <a name="registerpackageoptions"></a>[RegisterPackageOptions](https://docs.microsoft.com/uwp/api/windows.management.deployment.registerpackageoptions)

RegisterPackageOptions <br> RegisterPackageOptions.#ctor <br> RegisterPackageOptions.AllowUnsigned <br> RegisterPackageOptions.AppDataVolume <br> RegisterPackageOptions.DeferRegistrationWhenPackagesAreInUse <br> RegisterPackageOptions.DependencyPackageUris <br> RegisterPackageOptions.DeveloperMode <br> RegisterPackageOptions.ExternalLocationUri <br> RegisterPackageOptions.ForceAppShutdown <br> RegisterPackageOptions.ForceTargetAppShutdown <br> RegisterPackageOptions.ForceUpdateFromAnyVersion <br> RegisterPackageOptions.InstallAllResources <br> RegisterPackageOptions.OptionalPackageFamilyNames <br> RegisterPackageOptions.StageInPlace

#### <a name="stagepackageoptions"></a>[StagePackageOptions](https://docs.microsoft.com/uwp/api/windows.management.deployment.stagepackageoptions)

StagePackageOptions <br> StagePackageOptions.#ctor <br> StagePackageOptions.AllowUnsigned <br> StagePackageOptions.DependencyPackageUris <br> StagePackageOptions.DeveloperMode <br> StagePackageOptions.ExternalLocationUri <br> StagePackageOptions.ForceUpdateFromAnyVersion <br> StagePackageOptions.InstallAllResources <br> StagePackageOptions.OptionalPackageFamilyNames <br> StagePackageOptions.OptionalPackageUris <br> StagePackageOptions.RelatedPackageUris <br> StagePackageOptions.RequiredContentGroupOnly <br> StagePackageOptions.StageInPlace <br> StagePackageOptions.StubPackageOption <br> StagePackageOptions.TargetVolume

#### <a name="stubpackageoption"></a>[StubPackageOption](https://docs.microsoft.com/uwp/api/windows.management.deployment.stubpackageoption)

StubPackageOption

## <a name="windowsmedia"></a>Windows.Media

### <a name="windowsmediaaudio"></a>[Windows.Media.Audio](https://docs.microsoft.com/uwp/api/windows.media.audio)

#### <a name="audioplaybackconnection"></a>[AudioPlaybackConnection](https://docs.microsoft.com/uwp/api/windows.media.audio.audioplaybackconnection)

AudioPlaybackConnection <br> AudioPlaybackConnection.Close <br> AudioPlaybackConnection.DeviceId <br> AudioPlaybackConnection.GetDeviceSelector <br> AudioPlaybackConnection.Open <br> AudioPlaybackConnection.OpenAsync <br> AudioPlaybackConnection.Start <br> AudioPlaybackConnection.StartAsync <br> AudioPlaybackConnection.State <br> AudioPlaybackConnection.StateChanged <br> AudioPlaybackConnection.TryCreateFromId

#### <a name="audioplaybackconnectionopenresult"></a>[AudioPlaybackConnectionOpenResult](https://docs.microsoft.com/uwp/api/windows.media.audio.audioplaybackconnectionopenresult)

AudioPlaybackConnectionOpenResult <br> AudioPlaybackConnectionOpenResult.ExtendedError <br> AudioPlaybackConnectionOpenResult.Status

#### <a name="audioplaybackconnectionopenresultstatus"></a>[AudioPlaybackConnectionOpenResultStatus](https://docs.microsoft.com/uwp/api/windows.media.audio.audioplaybackconnectionopenresultstatus)

AudioPlaybackConnectionOpenResultStatus

#### <a name="audioplaybackconnectionstate"></a>[AudioPlaybackConnectionState](https://docs.microsoft.com/uwp/api/windows.media.audio.audioplaybackconnectionstate)

AudioPlaybackConnectionState

### <a name="windowsmediacaptureframes"></a>[Windows.Media.Capture.Frames](https://docs.microsoft.com/uwp/api/windows.media.capture.frames)

#### <a name="mediaframesourceinfo"></a>[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)

MediaFrameSourceInfo.GetRelativePanel

### <a name="windowsmediacapture"></a>[Windows.Media.Capture](https://docs.microsoft.com/uwp/api/windows.media.capture)

#### <a name="mediacapture"></a>[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)

MediaCapture.CreateRelativePanelWatcher

#### <a name="mediacaptureinitializationsettings"></a>[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)

MediaCaptureInitializationSettings.DeviceUri <br> MediaCaptureInitializationSettings.DeviceUriPasswordCredential

#### <a name="mediacapturerelativepanelwatcher"></a>[MediaCaptureRelativePanelWatcher](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturerelativepanelwatcher)

MediaCaptureRelativePanelWatcher <br> MediaCaptureRelativePanelWatcher.Changed <br> MediaCaptureRelativePanelWatcher.Close <br> MediaCaptureRelativePanelWatcher.RelativePanel <br> MediaCaptureRelativePanelWatcher.Start <br> MediaCaptureRelativePanelWatcher.Stop

### <a name="windowsmediadevices"></a>[Windows.Media.Devices](https://docs.microsoft.com/uwp/api/windows.media.devices)

#### <a name="panelbasedoptimizationcontrol"></a>[PanelBasedOptimizationControl](https://docs.microsoft.com/uwp/api/windows.media.devices.panelbasedoptimizationcontrol)

PanelBasedOptimizationControl <br> PanelBasedOptimizationControl.IsSupported <br> PanelBasedOptimizationControl.Panel

#### <a name="videodevicecontroller"></a>[VideoDeviceController](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller)

VideoDeviceController.PanelBasedOptimizationControl

### <a name="windowsmediamediaproperties"></a>[Windows.Media.MediaProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties)

#### <a name="mediaencodingsubtypes"></a>[MediaEncodingSubtypes](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)

MediaEncodingSubtypes.Pgs <br> MediaEncodingSubtypes.Srt <br> MediaEncodingSubtypes.Ssa <br> MediaEncodingSubtypes.VobSub

#### <a name="timedmetadataencodingproperties"></a>[TimedMetadataEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties)

TimedMetadataEncodingProperties.CreatePgs <br> TimedMetadataEncodingProperties.CreateSrt <br> TimedMetadataEncodingProperties.CreateSsa <br> TimedMetadataEncodingProperties.CreateVobSub

## <a name="windowsnetworking"></a>Windows.Networking

### <a name="windowsnetworkingbackgroundtransfer"></a>[Windows.Networking.BackgroundTransfer](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer)

#### <a name="downloadoperation"></a>[DownloadOperation](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.downloadoperation)

DownloadOperation.RemoveRequestHeader <br> DownloadOperation.SetRequestHeader

#### <a name="uploadoperation"></a>[UploadOperation](https://docs.microsoft.com/uwp/api/windows.networking.backgroundtransfer.uploadoperation)

UploadOperation.RemoveRequestHeader <br> UploadOperation.SetRequestHeader

### <a name="windowsnetworkingnetworkoperators"></a>[Windows.Networking.NetworkOperators](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators)

#### <a name="networkoperatortetheringaccesspointconfiguration"></a>[NetworkOperatorTetheringAccessPointConfiguration](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators.networkoperatortetheringaccesspointconfiguration)

NetworkOperatorTetheringAccessPointConfiguration.Band <br> NetworkOperatorTetheringAccessPointConfiguration.IsBandSupported <br> NetworkOperatorTetheringAccessPointConfiguration.IsBandSupportedAsync

#### <a name="networkoperatortetheringmanager"></a>[NetworkOperatorTetheringManager](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators.networkoperatortetheringmanager)

NetworkOperatorTetheringManager.DisableNoConnectionsTimeout <br> NetworkOperatorTetheringManager.DisableNoConnectionsTimeoutAsync <br> NetworkOperatorTetheringManager.EnableNoConnectionsTimeout <br> NetworkOperatorTetheringManager.EnableNoConnectionsTimeoutAsync <br> NetworkOperatorTetheringManager.IsNoConnectionsTimeoutEnabled

#### <a name="tetheringwifiband"></a>[TetheringWiFiBand](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators.tetheringwifiband)

TetheringWiFiBand

### <a name="windowsnetworkingpushnotifications"></a>[Windows.Networking.PushNotifications](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications)

#### <a name="rawnotification"></a>[RawNotification](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.rawnotification)

RawNotification.ContentBytes

## <a name="windowssecurity"></a>Windows.Security

### <a name="windowssecurityauthenticationwebcore"></a>[Windows.Security.Authentication.Web.Core](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core)

#### <a name="webaccountmonitor"></a>[WebAccountMonitor](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webaccountmonitor)

WebAccountMonitor.AccountPictureUpdated

### <a name="windowssecurityisolation"></a>[Windows.Security.Isolation](https://docs.microsoft.com/uwp/api/windows.security.isolation)

#### <a name="isolatedwindowsenvironment"></a>[IsolatedWindowsEnvironment](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironment)

IsolatedWindowsEnvironment <br> IsolatedWindowsEnvironment.CreateAsync <br> IsolatedWindowsEnvironment.CreateAsync <br> IsolatedWindowsEnvironment.FindByOwnerId <br> IsolatedWindowsEnvironment.GetById <br> IsolatedWindowsEnvironment.Id <br> IsolatedWindowsEnvironment.LaunchFileWithUIAsync <br> IsolatedWindowsEnvironment.LaunchFileWithUIAsync <br> IsolatedWindowsEnvironment.RegisterMessageReceiver <br> IsolatedWindowsEnvironment.ShareFolderAsync <br> IsolatedWindowsEnvironment.ShareFolderAsync <br> IsolatedWindowsEnvironment.StartProcessSilentlyAsync <br> IsolatedWindowsEnvironment.StartProcessSilentlyAsync <br> IsolatedWindowsEnvironment.TerminateAsync <br> IsolatedWindowsEnvironment.TerminateAsync

#### <a name="isolatedwindowsenvironment"></a>[IsolatedWindowsEnvironment](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentactivator)

IsolatedWindowsEnvironment.UnregisterMessageReceiver

#### <a name="isolatedwindowsenvironmentactivator"></a>[IsolatedWindowsEnvironmentActivator](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentallowedclipboardformats)

IsolatedWindowsEnvironmentActivator

#### <a name="isolatedwindowsenvironmentallowedclipboardformats"></a>[IsolatedWindowsEnvironmentAllowedClipboardFormats](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentavailableprinters)

IsolatedWindowsEnvironmentAllowedClipboardFormats

#### <a name="isolatedwindowsenvironmentavailableprinters"></a>[IsolatedWindowsEnvironmentAvailablePrinters](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentclipboardcopypastedirections)

IsolatedWindowsEnvironmentAvailablePrinters

#### <a name="isolatedwindowsenvironmentclipboardcopypastedirections"></a>[IsolatedWindowsEnvironmentClipboardCopyPasteDirections](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentcreateprogress)

IsolatedWindowsEnvironmentClipboardCopyPasteDirections

#### <a name="isolatedwindowsenvironmentcreateprogress"></a>[IsolatedWindowsEnvironmentCreateProgress](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentcreateresult)

IsolatedWindowsEnvironmentCreateProgress <br> IsolatedWindowsEnvironmentCreateResult <br> IsolatedWindowsEnvironmentCreateResult.Environment <br> IsolatedWindowsEnvironmentCreateResult.ExtendedError

#### <a name="isolatedwindowsenvironmentcreateresult"></a>[IsolatedWindowsEnvironmentCreateResult](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentcreatestatus)

IsolatedWindowsEnvironmentCreateResult.Status

#### <a name="isolatedwindowsenvironmentcreatestatus"></a>[IsolatedWindowsEnvironmentCreateStatus](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentfile)

IsolatedWindowsEnvironmentCreateStatus <br> IsolatedWindowsEnvironmentFile <br> IsolatedWindowsEnvironmentFile.Close <br> IsolatedWindowsEnvironmentFile.HostPath

#### <a name="isolatedwindowsenvironmentfile"></a>[IsolatedWindowsEnvironmentFile](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmenthost)

IsolatedWindowsEnvironmentFile.Id <br> IsolatedWindowsEnvironmentHost <br> IsolatedWindowsEnvironmentHost.HostErrors

#### <a name="isolatedwindowsenvironmenthost"></a>[IsolatedWindowsEnvironmentHost](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmenthosterror)

IsolatedWindowsEnvironmentHost.IsReady

#### <a name="isolatedwindowsenvironmenthosterror"></a>[IsolatedWindowsEnvironmentHostError](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentlaunchfileresult)

IsolatedWindowsEnvironmentHostError <br> IsolatedWindowsEnvironmentLaunchFileResult <br> IsolatedWindowsEnvironmentLaunchFileResult.ExtendedError <br> IsolatedWindowsEnvironmentLaunchFileResult.File

#### <a name="isolatedwindowsenvironmentlaunchfileresult"></a>[IsolatedWindowsEnvironmentLaunchFileResult](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentlaunchfilestatus)

IsolatedWindowsEnvironmentLaunchFileResult.Status

#### <a name="isolatedwindowsenvironmentlaunchfilestatus"></a>[IsolatedWindowsEnvironmentLaunchFileStatus](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentoptions)

IsolatedWindowsEnvironmentLaunchFileStatus <br> IsolatedWindowsEnvironmentOptions <br> IsolatedWindowsEnvironmentOptions.#ctor <br> IsolatedWindowsEnvironmentOptions.AllowCameraAndMicrophoneAccess <br> IsolatedWindowsEnvironmentOptions.AllowGraphicsHardwareAcceleration <br> IsolatedWindowsEnvironmentOptions.AllowedClipboardFormats <br> IsolatedWindowsEnvironmentOptions.AvailablePrinters <br> IsolatedWindowsEnvironmentOptions.ClipboardCopyPasteDirections <br> IsolatedWindowsEnvironmentOptions.EnvironmentOwnerId <br> IsolatedWindowsEnvironmentOptions.PersistUserProfile <br> IsolatedWindowsEnvironmentOptions.ShareHostFolderForUntrustedItems <br> IsolatedWindowsEnvironmentOptions.SharedFolderNameInEnvironment

#### <a name="isolatedwindowsenvironmentoptions"></a>[IsolatedWindowsEnvironmentOptions](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentownerregistration)

IsolatedWindowsEnvironmentOptions.SharedHostFolderPath <br> IsolatedWindowsEnvironmentOwnerRegistration <br> IsolatedWindowsEnvironmentOwnerRegistration.Register

#### <a name="isolatedwindowsenvironmentownerregistration"></a>[IsolatedWindowsEnvironmentOwnerRegistration](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentownerregistrationdata)

IsolatedWindowsEnvironmentOwnerRegistration.Unregister <br> IsolatedWindowsEnvironmentOwnerRegistrationData <br> IsolatedWindowsEnvironmentOwnerRegistrationData.#ctor <br> IsolatedWindowsEnvironmentOwnerRegistrationData.ActivationFileExtensions <br> IsolatedWindowsEnvironmentOwnerRegistrationData.ProcessesRunnableAsSystem <br> IsolatedWindowsEnvironmentOwnerRegistrationData.ProcessesRunnableAsUser

#### <a name="isolatedwindowsenvironmentownerregistrationdata"></a>[IsolatedWindowsEnvironmentOwnerRegistrationData](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentownerregistrationresult)

IsolatedWindowsEnvironmentOwnerRegistrationData.ShareableFolders <br> IsolatedWindowsEnvironmentOwnerRegistrationResult <br> IsolatedWindowsEnvironmentOwnerRegistrationResult.ExtendedError

#### <a name="isolatedwindowsenvironmentownerregistrationresult"></a>[IsolatedWindowsEnvironmentOwnerRegistrationResult](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentownerregistrationstatus)

IsolatedWindowsEnvironmentOwnerRegistrationResult.Status

#### <a name="isolatedwindowsenvironmentownerregistrationstatus"></a>[IsolatedWindowsEnvironmentOwnerRegistrationStatus](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentprocess)

IsolatedWindowsEnvironmentOwnerRegistrationStatus <br> IsolatedWindowsEnvironmentProcess <br> IsolatedWindowsEnvironmentProcess.ExitCode <br> IsolatedWindowsEnvironmentProcess.State <br> IsolatedWindowsEnvironmentProcess.WaitForExit <br> IsolatedWindowsEnvironmentProcess.WaitForExitAsync

#### <a name="isolatedwindowsenvironmentprocess"></a>[IsolatedWindowsEnvironmentProcess](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentprocessstate)

IsolatedWindowsEnvironmentProcess.WaitForExitWithTimeout

#### <a name="isolatedwindowsenvironmentprocessstate"></a>[IsolatedWindowsEnvironmentProcessState](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentprogressstate)

IsolatedWindowsEnvironmentProcessState

#### <a name="isolatedwindowsenvironmentprogressstate"></a>[IsolatedWindowsEnvironmentProgressState](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentsharefolderrequestoptions)

IsolatedWindowsEnvironmentProgressState <br> IsolatedWindowsEnvironmentShareFolderRequestOptions <br> IsolatedWindowsEnvironmentShareFolderRequestOptions.#ctor

#### <a name="isolatedwindowsenvironmentsharefolderrequestoptions"></a>[IsolatedWindowsEnvironmentShareFolderRequestOptions](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentsharefolderresult)

IsolatedWindowsEnvironmentShareFolderRequestOptions.AllowWrite <br> IsolatedWindowsEnvironmentShareFolderResult <br> IsolatedWindowsEnvironmentShareFolderResult.ExtendedError

#### <a name="isolatedwindowsenvironmentsharefolderresult"></a>[IsolatedWindowsEnvironmentShareFolderResult](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentsharefolderstatus)

IsolatedWindowsEnvironmentShareFolderResult.Status

#### <a name="isolatedwindowsenvironmentsharefolderstatus"></a>[IsolatedWindowsEnvironmentShareFolderStatus](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentstartprocessresult)

IsolatedWindowsEnvironmentShareFolderStatus <br> IsolatedWindowsEnvironmentStartProcessResult <br> IsolatedWindowsEnvironmentStartProcessResult.ExtendedError <br> IsolatedWindowsEnvironmentStartProcessResult.Process

#### <a name="isolatedwindowsenvironmentstartprocessresult"></a>[IsolatedWindowsEnvironmentStartProcessResult](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmentstartprocessstatus)

IsolatedWindowsEnvironmentStartProcessResult.Status

#### <a name="isolatedwindowsenvironmentstartprocessstatus"></a>[IsolatedWindowsEnvironmentStartProcessStatus](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowsenvironmenttelemetryparameters)

IsolatedWindowsEnvironmentStartProcessStatus <br> IsolatedWindowsEnvironmentTelemetryParameters <br> IsolatedWindowsEnvironmentTelemetryParameters.#ctor

#### <a name="isolatedwindowsenvironmenttelemetryparameters"></a>[IsolatedWindowsEnvironmentTelemetryParameters](https://docs.microsoft.com/uwp/api/windows.security.isolation.isolatedwindowshostmessenger)

IsolatedWindowsEnvironmentTelemetryParameters.CorrelationId <br> IsolatedWindowsHostMessenger <br> IsolatedWindowsHostMessenger.GetFileId

#### <a name="isolatedwindowshostmessenger"></a>[IsolatedWindowsHostMessenger](https://docs.microsoft.com/uwp/api/windows.security.isolation.messagereceivedcallback)

IsolatedWindowsHostMessenger.PostMessageToReceiver

#### <a name="messagereceivedcallback"></a>[MessageReceivedCallback](https://docs.microsoft.com/uwp/api/windows.security.isolation.windows)

MessageReceivedCallback

## <a name="windowsstorage"></a>Windows.Storage

### <a name="windowsstorageprovider"></a>[Windows.Storage.Provider](https://docs.microsoft.com/uwp/api/windows.storage.provider)

#### <a name="storageprovidersyncrootmanager"></a>[StorageProviderSyncRootManager](https://docs.microsoft.com/uwp/api/windows.storage.provider.storageprovidersyncrootmanager)

StorageProviderSyncRootManager.IsSupported

### <a name="windowsstorage"></a>[Windows.Storage](https://docs.microsoft.com/uwp/api/windows.storage)

#### <a name="knownfolders"></a>[KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders)

KnownFolders.GetFolderAsync <br> KnownFolders.RequestAccessAsync <br> KnownFolders.RequestAccessForUserAsync

#### <a name="knownfoldersaccessstatus"></a>[KnownFoldersAccessStatus](https://docs.microsoft.com/uwp/api/windows.storage.knownfoldersaccessstatus)

KnownFoldersAccessStatus

#### <a name="storagefile"></a>[StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)

StorageFile.GetFileFromPathForUserAsync

#### <a name="storagefolder"></a>[StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder)

StorageFolder.GetFolderFromPathForUserAsync

## <a name="windowssystem"></a>Windows.System

### <a name="windowssystem"></a>[Windows.System](https://docs.microsoft.com/uwp/api/windows.system)

#### <a name="userchangedeventargs"></a>[UserChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.system.userchangedeventargs)

UserChangedEventArgs.ChangedPropertyKinds

#### <a name="userwatcherupdatekind"></a>[UserWatcherUpdateKind](https://docs.microsoft.com/uwp/api/windows.system.userwatcherupdatekind)

UserWatcherUpdateKind

## <a name="windowsui"></a>Windows.UI

### <a name="windowsuicompositioninteractions"></a>[Windows.UI.Composition.Interactions](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions)

#### <a name="interactiontracker"></a>[InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker)

InteractionTracker.TryUpdatePosition

#### <a name="interactiontrackerpositionupdateoption"></a>[InteractionTrackerPositionUpdateOption](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontrackerpositionupdateoption)

InteractionTrackerPositionUpdateOption

### <a name="windowsuicorepreviewcommunications"></a>[Windows.UI.Core.Preview.Communications](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications)

#### <a name="previewmeetinginfodisplaykind"></a>[PreviewMeetingInfoDisplayKind](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewsystemstate)

PreviewMeetingInfoDisplayKind

#### <a name="previewsystemstate"></a>[PreviewSystemState](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewteamcleanuprequestedeventargs)

PreviewSystemState <br> PreviewTeamCleanupRequestedEventArgs

#### <a name="previewteamcleanuprequestedeventargs"></a>[PreviewTeamCleanupRequestedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewteamcommandinvokedeventargs)

PreviewTeamCleanupRequestedEventArgs.GetDeferral <br> PreviewTeamCommandInvokedEventArgs

#### <a name="previewteamcommandinvokedeventargs"></a>[PreviewTeamCommandInvokedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewteamdevicecredentials)

PreviewTeamCommandInvokedEventArgs.Command <br> PreviewTeamDeviceCredentials <br> PreviewTeamDeviceCredentials.#ctor <br> PreviewTeamDeviceCredentials.DomainUserName <br> PreviewTeamDeviceCredentials.Password

#### <a name="previewteamdevicecredentials"></a>[PreviewTeamDeviceCredentials](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewteamendmeetingkind)

PreviewTeamDeviceCredentials.UserPrincipalName

#### <a name="previewteamendmeetingkind"></a>[PreviewTeamEndMeetingKind](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewteamendmeetingrequestedeventargs)

PreviewTeamEndMeetingKind <br> PreviewTeamEndMeetingRequestedEventArgs

#### <a name="previewteamendmeetingrequestedeventargs"></a>[PreviewTeamEndMeetingRequestedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewteamjoinmeetingrequestedeventargs)

PreviewTeamEndMeetingRequestedEventArgs.GetDeferral <br> PreviewTeamJoinMeetingRequestedEventArgs <br> PreviewTeamJoinMeetingRequestedEventArgs.GetDeferral

#### <a name="previewteamjoinmeetingrequestedeventargs"></a>[PreviewTeamJoinMeetingRequestedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewteamview)

PreviewTeamJoinMeetingRequestedEventArgs.MeetingUri <br> PreviewTeamView <br> PreviewTeamView.CleanupRequested <br> PreviewTeamView.CommandInvoked <br> PreviewTeamView.EndMeetingRequested <br> PreviewTeamView.EnterFullScreen <br> PreviewTeamView.GetForCurrentView <br> PreviewTeamView.IsFullScreen <br> PreviewTeamView.IsFullScreenChanged <br> PreviewTeamView.IsScreenSharing <br> PreviewTeamView.IsScreenSharingChanged <br> PreviewTeamView.JoinMeetingRequested <br> PreviewTeamView.JoinMeetingWithUri <br> PreviewTeamView.LeaveFullScreen <br> PreviewTeamView.MeetingInfoDisplayType <br> PreviewTeamView.MeetingUri <br> PreviewTeamView.NotifyMeetingEnded <br> PreviewTeamView.RequestForeground <br> PreviewTeamView.SetButtonLabel <br> PreviewTeamView.SetTitle <br> PreviewTeamView.SharingScreenBounds <br> PreviewTeamView.SharingScreenBoundsChanged <br> PreviewTeamView.StartSharingScreen <br> PreviewTeamView.StopSharingScreen <br> PreviewTeamView.SystemState

#### <a name="previewteamview"></a>[PreviewTeamView](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.previewteamviewcommand)

PreviewTeamView.SystemStateChanged

#### <a name="previewteamviewcommand"></a>[PreviewTeamViewCommand](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.communications.windows)

PreviewTeamViewCommand

### <a name="windowsuiinputinking"></a>[Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)

#### <a name="inkmodelerattributes"></a>[InkModelerAttributes](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmodelerattributes)

InkModelerAttributes.UseVelocityBasedPressure

### <a name="windowsuiinput"></a>[Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)

#### <a name="crossslidingeventargs"></a>[CrossSlidingEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.crossslidingeventargs)

CrossSlidingEventArgs.ContactCount

#### <a name="draggingeventargs"></a>[DraggingEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.draggingeventargs)

DraggingEventArgs.ContactCount

#### <a name="gesturerecognizer"></a>[GestureRecognizer](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer)

GestureRecognizer.HoldMaxContactCount <br> GestureRecognizer.HoldMinContactCount <br> GestureRecognizer.HoldRadius <br> GestureRecognizer.HoldStartDelay <br> GestureRecognizer.TapMaxContactCount <br> GestureRecognizer.TapMinContactCount <br> GestureRecognizer.TranslationMaxContactCount <br> GestureRecognizer.TranslationMinContactCount

#### <a name="holdingeventargs"></a>[HoldingEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.holdingeventargs)

HoldingEventArgs.ContactCount <br> HoldingEventArgs.CurrentContactCount

#### <a name="manipulationcompletedeventargs"></a>[ManipulationCompletedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.manipulationcompletedeventargs)

ManipulationCompletedEventArgs.ContactCount <br> ManipulationCompletedEventArgs.CurrentContactCount

#### <a name="manipulationinertiastartingeventargs"></a>[ManipulationInertiaStartingEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.manipulationinertiastartingeventargs)

ManipulationInertiaStartingEventArgs.ContactCount

#### <a name="manipulationstartedeventargs"></a>[ManipulationStartedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.manipulationstartedeventargs)

ManipulationStartedEventArgs.ContactCount

#### <a name="manipulationupdatedeventargs"></a>[ManipulationUpdatedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.manipulationupdatedeventargs)

ManipulationUpdatedEventArgs.ContactCount <br> ManipulationUpdatedEventArgs.CurrentContactCount

#### <a name="righttappedeventargs"></a>[RightTappedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.righttappedeventargs)

RightTappedEventArgs.ContactCount

#### <a name="tappedeventargs"></a>[TappedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.systembuttoneventcontroller)

TappedEventArgs.ContactCount <br> ichEditMathMode <br> ichEditTextDocument.GetMath <br> ichEditTextDocument.SetMath <br> ichEditTextDocument.SetMathMode <br> nagement.Core.CoreInputView.PrimaryViewHiding

#### <a name="nagement"></a>[nagement](https://docs.microsoft.com/uwp/api/windows.ui.input.systemfunctionbuttoneventargs)

nagement.Core.CoreInputView.PrimaryViewShowing <br> nagement.Core.CoreInputViewHidingEventArgs <br> nagement.Core.CoreInputViewHidingEventArgs.TryCancel

#### <a name="nagement"></a>[nagement](https://docs.microsoft.com/uwp/api/windows.ui.input.systemfunctionlockchangedeventargs)

nagement.Core.CoreInputViewShowingEventArgs <br> nagement.Core.CoreInputViewShowingEventArgs.TryCancel <br> nagement.Core.UISettingsController <br> nagement.Core.UISettingsController.RequestDefaultAsync

#### <a name="nagement"></a>[nagement](https://docs.microsoft.com/uwp/api/windows.ui.input.systemfunctionlockindicatorchangedeventargs)

nagement.Core.UISettingsController.SetAdvancedEffectsEnabled <br> nagement.Core.UISettingsController.SetAnimationsEnabled <br> nagement.Core.UISettingsController.SetAutoHideScrollBars <br> nagement.Core.UISettingsController.SetMessageDuration

#### <a name="nagement"></a>[nagement](https://docs.microsoft.com/uwp/api/windows.ui.input.tappedeventargs)

nagement.Core.UISettingsController.SetTextScaleFactor

### <a name="windowsuiviewmanagement"></a>[Windows.UI.ViewManagement](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement)

#### <a name="uisettings"></a>[UISettings](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings)

UISettings.AnimationsEnabledChanged <br> UISettings.MessageDurationChanged

#### <a name="uisettingsanimationsenabledchangedeventargs"></a>[UISettingsAnimationsEnabledChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettingsanimationsenabledchangedeventargs)

UISettingsAnimationsEnabledChangedEventArgs

#### <a name="uisettingsmessagedurationchangedeventargs"></a>[UISettingsMessageDurationChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettingsmessagedurationchangedeventargs)

UISettingsMessageDurationChangedEventArgs