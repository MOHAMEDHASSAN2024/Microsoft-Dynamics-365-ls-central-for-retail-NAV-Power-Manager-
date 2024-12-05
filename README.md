# Microsoft-Dynamics-365-ls-central-for-retail-NAV-Power-Manager-
Microsoft Dynamics 365 ls central for retail NAV Power Manager 
مغامرة موظف الـ IT مع NAVPowerManager
في يوم من الأيام، كان فيه موظف IT اسمه حسام. حسام كان شاب مش عارف ينام الليل بسبب مشاكل في Microsoft Dynamics NAV. كان كلما حاول يدخل على النظام، يواجه مشاكل مع قاعدة البيانات، المستخدمين، وأحيانًا كان يضطر يدور على ملفات الترخيص الضايعة. كان كل شيء معقد وأصعب من لعبة شطرنج بالنسبة له.

لكن حسام قرر إنه لازم يلاقي حل سريع، وكان عنده حلم بسيط: "لو قدرت أخلي كل شيء أوتوماتيكي، هيبقى الدنيا سهلة!"

الخطوة الأولى: تعيين سياسة التنفيذ
أول خطوة حسام عملها كانت تعيين سياسة التنفيذ ليتمكن من تشغيل السكربتات من غير مشاكل.

powershell
Copy code
Set-ExecutionPolicy Unrestricted -Force
Write-Host "Execution Policy set to Unrestricted." -ForegroundColor Green
إيه الحكاية؟
دي ببساطة بتقول لـ PowerShell: "مسموح لك تشغّل أي سكربت، وما فيش أي قيود!". كده بقى حسام مستعد لتشغيل الأوامر اللي جايه من غير ما يعترضه أي حاجز.

الخطوة الثانية: استيراد NavAdminTool
الخطوة التانية كانت استيراد NavAdminTool، اللي هو أداة التحكم الأساسية لـ Dynamics NAV:

powershell
Copy code
Import-Module "C:\Program Files\Microsoft Dynamics 365 Business Central\210\Service\NavAdminTool.ps1"
Write-Host "NavAdminTool module imported successfully." -ForegroundColor Green
إيه الحكاية؟
الـ NavAdminTool ده زي السوبرمان بالنسبة لـ NAV. بيخلي حسام يتحكم في كل شيء، من إعدادات السيرفر لغاية الترخيص و المستخدمين، بشكل سريع وسهل.

الخطوة الثالثة: تعيين قاعدة البيانات وإعادة تشغيل السيرفر
حسام عاوز يغير قاعدة البيانات ويخلي السيرفر يشتغل على القاعدة الجديدة:

powershell
Copy code
Set-NAVServerConfiguration -KeyName "DatabaseName" -ServerInstance $ServerInstance -KeyValue $DatabaseName
Write-Host "Setting Database Name to $DatabaseName..." -ForegroundColor Yellow
Restart-NAVServerInstance -ServerInstance $ServerInstance -Verbose
إيه الحكاية؟
هنا حسام بيقول لـ NAV: "القاعدة اللي هشتغل عليها اسمها كذا!" وبعد ما يضبط كل شيء، يعيد تشغيل السيرفر علشان التغييرات تتم.

الخطوة الرابعة: تفعيل خدمات SOAP و ODATA و Developer
حسام كان لازم يفعّل خدمات SOAP و ODATA و Developer Services علشان يقدر يشغّل الواجهات البرمجية ويبني حلول مخصصة:

powershell
Copy code
Set-NAVServerConfiguration -KeyName "SOAPServicesEnabled" -ServerInstance $ServerInstance -KeyValue "True"
Set-NAVServerConfiguration -KeyName "ODATAServicesEnabled" -ServerInstance $ServerInstance -KeyValue "True"
Set-NAVServerConfiguration -KeyName "DeveloperServicesEnabled" -ServerInstance $ServerInstance -KeyValue "True"
Write-Host "Enabling SOAP, ODATA, and Developer Services..." -ForegroundColor Yellow
Restart-NAVServerInstance -ServerInstance $ServerInstance -Verbose
إيه الحكاية؟
ببساطة، حسام كان عاوز يشغل الخدمات اللي بتخلي النظام يتفاعل مع التطبيقات الخارجية، زي Web Services أو API. بعد ما فعلهم، عمل إعادة تشغيل علشان التغييرات تتم.

الخطوة الخامسة: استيراد الترخيص
حسام كان لازم يجيب الترخيص من مكانه، وبما إن حسام مش دايمًا فاكر مكان الملف، كان لازم يتأكد الأول إن الترخيص موجود:

powershell
Copy code
$DefaultLicensePath = "C:\license.bclicense"
if (Test-Path $DefaultLicensePath) {
    $LicenseFile = $DefaultLicensePath
    Write-Host "Using the license file found at: $LicenseFile" -ForegroundColor Green
} else {
    $LicenseFile = Read-Host "Please enter the full path to the license file"
    if (-Not (Test-Path $LicenseFile)) {
        Write-Host "The specified license file does not exist: $LicenseFile" -ForegroundColor Red
        exit
    }
}
Import-NAVServerLicense -ServerInstance $ServerInstance -LicenseFile $LicenseFile
Write-Host "License imported successfully." -ForegroundColor Green
إيه الحكاية؟
هنا حسام كان عاوز يستورد الترخيص علشان يشغل النظام. إذا الترخيص مش موجود، هيطلب من حسام إدخاله. وعشان ما يقع في فخ الملفات الضايعة، حسام بيشيك الأول إذا كان الترخيص موجود قبل ما يدرجه في النظام.

الخطوة السادسة: إنشاء المستخدم وتعيين الصلاحيات
وأخيرًا، حسام كان عاوز يضيف مستخدم جديد ويعطيه صلاحيات، وده كان الجزء الحاسم. بس بدل ما يحاول كل مرة يضيف مستخدم ويطلع له خطأ "المستخدم موجود بالفعل"، حسام عمل التالي:

powershell
Copy code
$userExists = Get-NAVServerUser -ServerInstance $ServerInstance | Where-Object { $_.WindowsAccount -eq $WindowsAccount }
if ($userExists) {
    Write-Host "User $WindowsAccount already exists." -ForegroundColor Yellow
} else {
    New-NAVServerUser -ServerInstance $ServerInstance -WindowsAccount $WindowsAccount
    Write-Host "NAV Server User $WindowsAccount created successfully." -ForegroundColor Green
}

Try {
    New-NAVServerUserPermissionSet -PermissionSetId $PermissionSet -ServerInstance $ServerInstance -WindowsAccount $WindowsAccount
    Write-Host "Permission Set '$PermissionSet' assigned successfully to $WindowsAccount." -ForegroundColor Green
} Catch {
    Write-Host "Permission Set '$PermissionSet' is already assigned or another error occurred." -ForegroundColor Yellow
}
إيه الحكاية؟
هنا حسام كان عاوز يضيف مستخدم جديد، لكن قبل ما يضيفه، بيشيك إذا كان موجود فعلاً ولا لأ. ولو كان موجود، بيكتب رسالة تحذير ويكمل. بعد كده يضيف الصلاحيات، وفي حالة وجود الصلاحية مسبقًا، السكربت بيتجاهل الخطأ ويكمل.

الخطوة الأخيرة: إعادة تشغيل السيرفر
وبعد كل العمليات دي، حسام كان لازم يعيد تشغيل السيرفر مرة تانية علشان كل التغييرات تطبق وتشتغل بشكل صحيح:

powershell
Copy code
Restart-NAVServerInstance -ServerInstance $ServerInstance -Verbose
Write-Host "All operations completed successfully!" -ForegroundColor Green
الخاتمة:
بعد ما حسام شغل NAVPowerManager، أصبح بطل في إدارة Dynamics NAV. مابقاش عنده مشاكل مع المستخدمين، أو الترخيص، أو قاعدة البيانات. كل حاجة بقت أوتوماتيكية بفضل السكربت ده!

ولما جه يوم التقارير، حسام كان مبتسم وقال:
"لو مش معايا NAVPowerManager، كنت هفضل مش لوحدي في عالم الأخطاء!"

