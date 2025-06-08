#Sentinel環境のパラメーター
$TenantName = ""
$SubscriptionId = ""
$ResourceGroupName = ""
$WorkspaceName = ""

#Microsoft Azureへのサインイン
Connect-AzAccount -tenant $TenantName

#URIの生成
$ResourcePath = "https://management.azure.com/subscriptions/$SubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/$WorkspaceName/providers/Microsoft.SecurityInsights"
$InstalledEndpoint = "contentPackages"
$AvailableEndpoint = "contentProductPackages"
$ApiVersionParam = "?api-version=2024-03-01"
$InstalledUri = "$ResourcePath/$InstalledEndpoint$ApiVersionParam"
$AvailableUri = "$ResourcePath/$AvailableEndpoint$ApiVersionParam"

#インストール済み/インストール可能なコンテンツハブ一覧の収集
$InstalledContent = Invoke-AzRestMethod -Method GET -Uri $InstalledUri
$InstalledSolutions = ($InstalledContent.content | ConvertFrom-Json).value
$AvailableContent = Invoke-AzRestMethod -Method GET -Uri $AvailableUri
$AvailableSolutions = ($AvailableContent.content | ConvertFrom-Json).value

#更新可能なコンテンツハブ一覧の表示
foreach ($Installed in $InstalledSolutions) {
  $Available = $AvailableSolutions | Where-Object {
    $PSITEM.properties.displayName -eq $Installed.properties.displayName
  }
  if ($Available.properties.version -gt $Installed.properties.version) {
    [PSCustomObject]@{
    DisplayName = $Installed.properties.displayName
    CurrentVersion = $Installed.properties.version
    AvailableVersion = $Available.properties.version
    ContentId = $Installed.properties.contentId
  }
}
}
