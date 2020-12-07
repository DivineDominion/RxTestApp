# Test Project to Demonstrate that archiving with RxCocoa fails

Check out the project:

- `git clone --recursive https://github.com/DivineDominion/RxTestApp.git`
- ... or initialize submodules after cloning `git submodule init; git submodule update`

Archiving a macOS app for distribution with RxCocoa fails on my machine. Here's how, and what it says.

Similar issues I found on the web:

- https://github.com/helpscout/beacon-ios-sdk/issues/113 (closed source): "The i386 architecture needs stripping from Beacon.framework if to be successfully submitted to the store."

## Steps to Reproduce

Showing results for both

- Xcode, and
- Fastlane

after cloning the repo.

### Manual / Xcode

2. Open Xcode project
3. Adjust the app target's code signing identity to yours
3. Make an "Archive" build in Xcode
4. Try to upload the app for notarization
    - In the Xcode Organizer that popped up, select the Xcode archive if needed
    - Select "Distribute App" to the right
    - Select "Developer ID" (non-App Store export); you can also select "App Store Connect", doesn't matter, just takes 2 clicks more -- hit "Next"
    - Select "Upload" for notarization, hit "Next"
    - Select "Automatically manage signing", hit "Next" (also fails if you select profiles manually, though)

Check the `IDEDistributionPipelines.log`:

```
...
2020-12-07 15:10:48 +0000  Skipping architecture thinning for item "RxTestApp" because arch "arm64e" wasn't found
2020-12-07 15:10:48 +0000  Processing step: IDEDistributionODRStep
2020-12-07 15:10:48 +0000  Processing step: IDEDistributionStripXattrsStep
2020-12-07 15:10:48 +0000  Skipping stripping extended attributes of item: <IDEDistributionItem: 0x7fd1b3698110; bundleID='io.rx.RxCocoa', path='<DVTFilePath:0x7fd24432b9e0:'/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 07.12.20, 16.10.xcarchive/Products/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A'>', codeSigningInfo='<_DVTCodeSigningInformation_Path: 0x7fd2200de2b0; isSigned='1', isAdHocSigned='0', signingCertificate='<DVTSigningCertificate: 0x7fd2266baa50; name='Apple Development: Christian Tietze (933RH59P6T)', hash='893EB53E4123EE2FBF9D7C6ED10005F804F2768E', serialNumber='<DVTSigningCertificateSerialNumber: 0x7fd186ffaf50>', certificateKinds='(
    "1.2.840.113635.100.6.1.12",
    "1.2.840.113635.100.6.1.2"
), issueDate='2020-09-21 08:10:09 +0000''>', entitlements='(null)', teamID='FRMDA3XRGC', identifier='io.rx.RxCocoa', executablePath='<DVTFilePath:0x7fd2370b1710:'/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 07.12.20, 16.10.xcarchive/Products/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A/RxCocoa'>', hardenedRuntime='1'>'>
2020-12-07 15:10:48 +0000  Skipping stripping extended attributes of item: <IDEDistributionItem: 0x7fd200ae2510; bundleID='io.rx.RxSwift', path='<DVTFilePath:0x7fd24433a010:'/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 07.12.20, 16.10.xcarchive/Products/Applications/RxTestApp.app/Contents/Frameworks/RxSwift.framework/Versions/A'>', codeSigningInfo='<_DVTCodeSigningInformation_Path: 0x7fd203c7cd00; isSigned='1', isAdHocSigned='0', signingCertificate='<DVTSigningCertificate: 0x7fd2266baa50; name='Apple Development: Christian Tietze (933RH59P6T)', hash='893EB53E4123EE2FBF9D7C6ED10005F804F2768E', serialNumber='<DVTSigningCertificateSerialNumber: 0x7fd186f7fca0>', certificateKinds='(
    "1.2.840.113635.100.6.1.12",
    "1.2.840.113635.100.6.1.2"
), issueDate='2020-09-21 08:10:09 +0000''>', entitlements='(null)', teamID='FRMDA3XRGC', identifier='io.rx.RxSwift', executablePath='<DVTFilePath:0x7fd200cd7140:'/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 07.12.20, 16.10.xcarchive/Products/Applications/RxTestApp.app/Contents/Frameworks/RxSwift.framework/Versions/A/RxSwift'>', hardenedRuntime='1'>'>
2020-12-07 15:10:48 +0000  Running /usr/bin/xattr '-crs' '/var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/XcodeDistPipeline.~~~30iivY/Root/Applications/RxTestApp.app'
2020-12-07 15:10:49 +0000  /usr/bin/xattr exited with 0
2020-12-07 15:10:49 +0000  Processing step: IDEDistributionCodesignStep
2020-12-07 15:10:49 +0000  Entitlements for <IDEDistributionItem: 0x7fd1b3698110; bundleID='io.rx.RxCocoa', path='<DVTFilePath:0x7fd24432b9e0:'/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 07.12.20, 16.10.xcarchive/Products/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A'>', codeSigningInfo='<_DVTCodeSigningInformation_Path: 0x7fd2200de2b0; isSigned='1', isAdHocSigned='0', signingCertificate='<DVTSigningCertificate: 0x7fd221196d50; name='Apple Development: Christian Tietze (933RH59P6T)', hash='893EB53E4123EE2FBF9D7C6ED10005F804F2768E', serialNumber='<DVTSigningCertificateSerialNumber: 0x7fd221456060>', certificateKinds='(
    "1.2.840.113635.100.6.1.12",
    "1.2.840.113635.100.6.1.2"
), issueDate='2020-09-21 08:10:09 +0000''>', entitlements='(null)', teamID='FRMDA3XRGC', identifier='io.rx.RxCocoa', executablePath='<DVTFilePath:0x7fd2370b1710:'/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 07.12.20, 16.10.xcarchive/Products/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A/RxCocoa'>', hardenedRuntime='1'>'>: {
}
2020-12-07 15:10:49 +0000  Writing entitlements for <IDEDistributionItem: 0x7fd1b3698110; bundleID='io.rx.RxCocoa', path='<DVTFilePath:0x7fd24432b9e0:'/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 07.12.20, 16.10.xcarchive/Products/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A'>', codeSigningInfo='<_DVTCodeSigningInformation_Path: 0x7fd2200de2b0; isSigned='1', isAdHocSigned='0', signingCertificate='<DVTSigningCertificate: 0x7fd2224a0da0; name='Apple Development: Christian Tietze (933RH59P6T)', hash='893EB53E4123EE2FBF9D7C6ED10005F804F2768E', serialNumber='<DVTSigningCertificateSerialNumber: 0x7fd1a668b350>', certificateKinds='(
    "1.2.840.113635.100.6.1.12",
    "1.2.840.113635.100.6.1.2"
), issueDate='2020-09-21 08:10:09 +0000''>', entitlements='(null)', teamID='FRMDA3XRGC', identifier='io.rx.RxCocoa', executablePath='<DVTFilePath:0x7fd2370b1710:'/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 07.12.20, 16.10.xcarchive/Products/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A/RxCocoa'>', hardenedRuntime='1'>'> to: /var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/XcodeDistPipeline.~~~30iivY/entitlements~~~AkwhDT
2020-12-07 15:10:49 +0000  Running /usr/bin/codesign '-vvv' '--force' '--sign' 'F1C8B9C025AADFD0F5A1C94704B713AE48E05B2D' '--entitlements' '/var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/XcodeDistPipeline.~~~30iivY/entitlements~~~AkwhDT' '--preserve-metadata=identifier,flags,runtime' '--requirements' '=designated => anchor apple generic  and identifier "$self.identifier" and ((cert leaf[field.1.2.840.113635.100.6.1.9] exists) or ( certificate 1[field.1.2.840.113635.100.6.2.6] exists and certificate leaf[field.1.2.840.113635.100.6.1.13] exists  and certificate leaf[subject.OU] = "FRMDA3XRGC" ))' '/var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/XcodeDistPipeline.~~~30iivY/Root/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A'
2020-12-07 15:10:49 +0000  /var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/XcodeDistPipeline.~~~30iivY/Root/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A: replacing existing signature
2020-12-07 15:10:49 +0000  /var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/XcodeDistPipeline.~~~30iivY/Root/Applications/RxTestApp.app/Contents/Frameworks/RxCocoa.framework/Versions/A: code object is not signed at all
2020-12-07 15:10:49 +0000  /usr/bin/codesign exited with 1
```

### Fastlane

If you have fastlane installed, you can run

    $ fastlane gym --verbose -j developer-id

from the project directory and should get a similar result but with less clicking.

```
...
INFO [2020-12-07 16:19:40.03]: ▸ + xcodebuild -exportArchive -exportOptionsPlist /var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/gym_config20201207-13937-p21pyh.plist -archivePath '/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 2020-12-07 16.18.57.xcarchive' -exportPath /var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/gym_output20201207-13937-13r0yka
INFO [2020-12-07 16:19:40.76]: ▸ 2020-12-07 16:19:40.760 xcodebuild[14561:2028770] [MT] IDEDistribution: -[IDEDistributionLogging _createLoggingBundleAtPath:]: Created bundle at path '/var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/RxTestApp_2020-12-07_16-19-40.759.xcdistributionlogs'.
INFO [2020-12-07 16:19:41.50]: ▸ error: exportArchive: Code signing "A" failed.
INFO [2020-12-07 16:19:41.50]: ▸ Error Domain=IDEDistributionPipelineErrorDomain Code=0 "Code signing "A" failed." UserInfo={NSLocalizedDescription=Code signing "A" failed., NSLocalizedRecoverySuggestion=View distribution logs for more information.}
INFO [2020-12-07 16:19:41.50]: ▸ ** EXPORT FAILED **
RVM detected, forcing to use system ruby
Now using system ruby.
+ xcodebuild -exportArchive -exportOptionsPlist /var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/gym_config20201207-13937-p21pyh.plist -archivePath '/Users/ctm/Library/Developer/Xcode/Archives/2020-12-07/RxTestApp 2020-12-07 16.18.57.xcarchive' -exportPath /var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/gym_output20201207-13937-13r0yka
2020-12-07 16:19:40.760 xcodebuild[14561:2028770] [MT] IDEDistribution: -[IDEDistributionLogging _createLoggingBundleAtPath:]: Created bundle at path '/var/folders/62/8k21681d08z9lhq8h433z3rh0000gp/T/RxTestApp_2020-12-07_16-19-40.759.xcdistributionlogs'.
error: exportArchive: Code signing "A" failed.

Error Domain=IDEDistributionPipelineErrorDomain Code=0 "Code signing "A" failed." UserInfo={NSLocalizedDescription=Code signing "A" failed., NSLocalizedRecoverySuggestion=View distribution logs for more information.}

** EXPORT FAILED **
ERROR [2020-12-07 16:19:41.51]: Exit status: 70
...
```

## License

This project: Public Domain / Unlicense.

RxSwift: The MIT License Copyright © 2015 Krunoslav Zaher, Shai Mishali All rights reserved.
