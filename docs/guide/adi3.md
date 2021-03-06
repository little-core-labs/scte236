The **mediaxml** module provides an implementation for the
**_Asset Delivery Interface 3 (ADI3)**_ XML format. The format is based on the
_SCTE-236 - Content Metadata Specification_

### Example Usage

_See [example ADI3 XML document](#adi3-example-xml-document)._

```js
const adi3 = require('mediaxml/adi3')
const fs = require('fs')

const stream = fs.createReadStream('package.xml')
const document = adi3.createDocument(stream)

document.ready(() => {
  console.log(document.metadata.appData) // ADI3 > Metadata > App_Data
  console.log(document.metadata.ams.provider) // ADI3 > Metadata > AMS > .provider })
```

### Basic API

The basic API is very similar to the other [MediaXML modules](#mediaxml-api).
The `mediaxml/adi` module provides a canonical `Document` class that is
the root of the ADI3 XML implementation. The various other classes
provided by the `mediaxml/adi` module have accessors for properties
defined by ADI3 that give normalized values in a convenient manner such
as a SMPTE timecode in an `Chaper` `timCode` attribute normalized to a
[`SMPTETimecde`](https://github.com/CrystalComputerCorp/smpte-timecode).

#### `Document`

A class that represents an ADI3 XML document.

```js
const { createDocument } = require('mediaxml/adi3')
const fs = require('fs')

const stream = fs.createReadStream('package.xml')
const document = createDocument(stream)

document.ready(() => {
  for (const asset of document.assets) {
    console.log(asset.type, asset.uriId, asset.presentation)
  }
})
```

##### See Also

* [ADI3 API](#adi3)

<a name="adi3-example-xml-document"></a>
### Example XML Document

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<ADI3
	xmlns="http://www.scte.org/schemas/236/2016/core"
	xmlns:content="http://www.scte.org/schemas/236/2016/content"
	xmlns:core="http://www.scte.org/schemas/236/2016/core"
	xmlns:offer="http://www.scte.org/schemas/236/2016/offer"
	xmlns:terms="http://www.scte.org/schemas/236/2016/terms"
	xmlns:title="http://www.scte.org/schemas/236/2016/title"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="urn:cablelabs:md:xsd:core:3.0 SCTE236-COREC02.xsd urn:cablelabs:md:xsd:offer:3.0 SCTE236-OFFER-C02.xsd urn:cablelabs:md:xsd:title:3.0 SCTE236-TITLE-C02.xsd urn:cablelabs:md:xsd:content:3.0 SCTE236-CONTENT-C02.xsd urn:cablelabs:md:xsd:terms:3.0 SCTE236-TERMS-C02.xsd"
>
	<Asset
		xsi:type="offer:OfferType"
		uriId="indemand.com/Offer/UNVA2001081701004000"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004000</core:AlternateId>
		<core:AssetName deprecated="true">The_Titanic</core:AssetName>
		<core:Product deprecated="true">First-Run</core:Product>
		<core:Provider>InDemand</core:Provider>
		<core:Description deprecated="true">The Titanic asset package</core:Description>
		<core:Ext/>
		<offer:Presentation>
			<offer:CategoryRef uriId="indemand.com/Category/InDemand/Movies A-Z"/>
			<offer:DisplayAsNew>P7D</offer:DisplayAsNew>
			<offer:DisplayAsLastChance>P7D</offer:DisplayAsLastChance>
		</offer:Presentation>
		<offer:PromotionalContentGroupRef uriId="indemand.com/ContentGroup/UNVA2001081701004001"/>
		<offer:ProviderContentTier>InDemand1</offer:ProviderContentTier>
		<offer:BillingId>56789</offer:BillingId>
		<offer:TermsRef uriId="indemand.com/Terms/UNVA2001081701004001"/>
		<offer:ContentGroupRef uriId="indemand.com/ContentGroup/UNVA2001081701004001"/>
	</Asset>

	<Asset
		xsi:type="title:TitleType"
		uriId="indemand.com/Title/UNVA2001081701004001"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004001</core:AlternateId>
		<core:AlternateId identifierSystem="ISAN">1881-66C7-3420-000-7-9F3A-02450- U</core:AlternateId>
		<core:ProviderQAContact>John Doe, JDoe@InDemand.com</core:ProviderQAContact>
		<core:AssetName deprecated="true">The_Titanic_Title</core:AssetName>
		<core:Product deprecated="true">First-Run</core:Product>
		<core:Provider>InDemand</core:Provider>
		<core:Description deprecated="true">The Titanic title metadata</core:Description>
		<core:Ext>
			<App_Data App="MOD" Name="Producer" Value="Cameron,James"/>
		</core:Ext>
		<title:LocalizableTitle>
			<title:TitleSortName>Titanic, The</title:TitleSortName>
			<title:TitleBrief>The Titanic</title:TitleBrief>
			<title:TitleMedium>The Titanic</title:TitleMedium>
			<title:TitleLong>The Titanic</title:TitleLong>
			<title:SummaryShort>Fictional romantic tale of a rich girl and poor boy who meet on the ill-fated voyage of the 'unsinkable' ship</title:SummaryShort>
			<title:SummaryMedium>Fictional romantic tale of rich girl and poor boy who meet on the ill-fated voyage of the 'unsinkable' ship</title:SummaryMedium>
			<title:SummaryLong>Fictional romantic tale of a rich girl and poor boy who meet on the ill-fated voyage of the 'unsinkable' ship</title:SummaryLong>
			<title:ActorDisplay>Kate Winslet,Leonardo DiCaprio, Billy Zane</title:ActorDisplay>
			<title:Actor fullName="Kate Winslet" firstName="Kate" lastName="Winslet" sortableName="Winslet,Kate"/>
			<title:Actor fullName="Leonardo DiCaprio" firstName="Leonardo" lastName="DiCaprio" sortableName="DiCaprio,Leonardo"/>
			<title:Actor fullName="Billy Zane" firstName="Billy" lastName="Zane" sortableName="Zane,Billy"/>
			<title:WriterDisplay>James Cameron</title:WriterDisplay>
			<title:Director fullName="James Cameron" firstName="James" lastName="Cameron" sortableName="Cameron,James"/>
			<title:StudioDisplay>Paramount</title:StudioDisplay>
			<title:RecordingArtist>Celine Dion</title:RecordingArtist>
			<title:SongTitle>My Heart Will Go On</title:SongTitle>
			<title:EpisodeName>Collision With destiny</title:EpisodeName>
			<title:EpisodeID>The only one</title:EpisodeID>
			<title:Chapter heading="Opening Scene" timeCode="00:00:02:00"/>
			<title:Chapter heading="They Meet" timeCode="00:15:12:00"/>
			<title:Chapter heading="The Collision" timeCode="02:03:06:00"/>
			<title:Chapter heading="She Goes Under" timeCode="02:55:15:00"/>
			<title:Chapter heading="Final Scene" timeCode="03:02:09:00"/>
		</title:LocalizableTitle>

		<title:Rating ratingSystem="MPAA">R</title:Rating>
		<title:Advisory>AL</title:Advisory>
		<title:Advisory>N</title:Advisory>
		<title:IsClosedCaptioning>false</title:IsClosedCaptioning>
		<title:DisplayRunTime>03:14</title:DisplayRunTime>
		<title:Year>1997</title:Year>
		<title:CountryOfOrigin>US</title:CountryOfOrigin>
		<title:Genre>Drama</title:Genre>
		<title:Genre>Romance</title:Genre>
		<title:ShowType>Movie</title:ShowType>
		<title:IsSeasonPremiere>false</title:IsSeasonPremiere>
		<title:IsSeasonFinale>false</title:IsSeasonFinale>
		<title:IsEncryptionRequired>true</title:IsEncryptionRequired>
		<title:BoxOffice>1835000000</title:BoxOffice>
		<title:ProgrammerCallLetters>IND</title:ProgrammerCallLetters>
	</Asset>

	<Asset
		xsi:type="offer:ContentGroupType"
		uriId="indemand.com/ContentGroup/UNVA2001081701004001"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004001</core:AlternateId>
		<offer:TitleRef uriId="indemand.com/Title/UNVA2001081701004001"/>
		<offer:MovieRef uriId="indemand.com/Asset/UNVA2001081701004002"/>
		<offer:PreviewRef uriId="indemand.com/Asset/UNVA2001081701004003"/>
		<offer:BoxCoverRef uriId="indemand.com/Asset/UNVA2001081701004004"/>
		<offer:MovieRef uriId="indemand.com/Asset/UNVA2001081701004005"/>
	</Asset>

	<Asset
		xsi:type="terms:TermsType"
		uriId="indemand.com/Terms/UNVA2001081701004001"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004001</core:AlternateId>
		<terms:ContractName>InDemand</terms:ContractName>
		<terms:BillingGracePeriod>PT300S</terms:BillingGracePeriod>
		<terms:UsagePeriod>P00DT24H00M</terms:UsagePeriod>
		<terms:HomeVideoWindow>P1000D</terms:HomeVideoWindow>
		<terms:SubscriberViewLimit startDateTime="2002-03-01T00:00:00Z" endDateTime="2002-03-31T23:59:59Z" maximumViews="5"/>
		<terms:SuggestedPrice>5.95</terms:SuggestedPrice>
		<terms:DistributorRoyaltyInfo>
			<terms:OrganizationName>InDemand</terms:OrganizationName>
			<terms:RoyaltyPercent>52.5</terms:RoyaltyPercent>
			<terms:RoyaltyMinimum>3.124</terms:RoyaltyMinimum>
			<terms:RoyaltyFlatRate>3.155</terms:RoyaltyFlatRate>
		</terms:DistributorRoyaltyInfo>
		<terms:StudioRoyaltyInfo>
			<terms:OrganizationName>Paramount</terms:OrganizationName>
			<terms:OrganizationCode>PAR</terms:OrganizationCode>
			<terms:RoyaltyPercent>47.5</terms:RoyaltyPercent>
			<terms:RoyaltyMinimum>2.826</terms:RoyaltyMinimum>
			<terms:RoyaltyFlatRate>2.795</terms:RoyaltyFlatRate>
		</terms:StudioRoyaltyInfo>
	</Asset>

	<Asset
		xsi:type="offer:CategoryType"
		uriId="indemand.com/Category/InDemand/Movies A-Z"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004001</core:AlternateId>
		<offer:CategoryPath>InDemand/Movies A-Z</offer:CategoryPath>
	</Asset>

	<Asset
		xsi:type="content:MovieType"
		uriId="indemand.com/Asset/UNVA2001081701004002"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004002</core:AlternateId>
		<core:AssetName deprecated="true">The_Titanic.mpg</core:AssetName>
		<core:Product deprecated="true">First-Run</core:Product>
		<core:Provider>InDemand</core:Provider>
		<core:Description deprecated="true">The Titanic Movie</core:Description>
		<core:Ext/>
		<content:SourceUrl>The_Titanic.mpg</content:SourceUrl>
		<content:ContentFileSize>3907840625</content:ContentFileSize>
		<content:ContentCheckSum>12558D3269D25852BD26548DC2654CA2</content:ContentCheckSum>
		<content:PropagationPriority>1</content:PropagationPriority>
		<content:AudioType>Dolby Digital</content:AudioType>
		<content:ScreenFormat>Widescreen</content:ScreenFormat>
		<content:Duration>PT03H14M00S</content:Duration>
		<content:Language>en</content:Language>
		<content:SubtitleLanguage>es</content:SubtitleLanguage>
		<content:DubbedLanguage>es</content:DubbedLanguage>
		<content:Rating ratingSystem="MPAA">R</content:Rating>
		<content:CopyControlInfo>
			<content:IsCopyProtection>false</content:IsCopyProtection>
		</content:CopyControlInfo>
	</Asset>

	<Asset
		xsi:type="content:PreviewType"
		uriId="indemand.com/Asset/UNVA2001081701004003"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004003</core:AlternateId>
		<core:AssetName deprecated="true">The_Titanic_Preview.mpg</core:AssetName>
		<core:Product deprecated="true">First-Run</core:Product>
		<core:Provider>InDemand</core:Provider>
		<core:Description deprecated="true">The Titanic Preview</core:Description>
		<core:Ext/>
		<content:SourceUrl>The_Titanic_Preview.mpg</content:SourceUrl>
		<content:ContentFileSize>25284375</content:ContentFileSize>
		<content:ContentCheckSum>A1258D3269D25852BD26548DC2654C12</content:ContentCheckSum>
		<content:PropagationPriority>1</content:PropagationPriority>
		<content:AudioType>Dolby Digital</content:AudioType>
		<content:ScreenFormat>Widescreen</content:ScreenFormat>
		<content:Duration>PT00H00M45S</content:Duration>
		<content:Language>en</content:Language>
		<content:SubtitleLanguage>es</content:SubtitleLanguage>
		<content:DubbedLanguage>es</content:DubbedLanguage>
		<content:Rating ratingSystem="MPAA">G</content:Rating>
	</Asset>

	<Asset
		xsi:type="content:BoxCoverType"
		uriId="indemand.com/Asset/UNVA2001081701004004"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004004</core:AlternateId>
		<core:AssetName deprecated="true">The_Titanic_Box_Cover.bmp</core:AssetName>
		<core:Product deprecated="true">First-Run</core:Product>
		<core:Provider>InDemand</core:Provider>
		<core:Description deprecated="true">The Titanic Box Cover</core:Description>
		<core:Ext/>
		<content:SourceUrl>The_Titanic_Box_Cover.bmp</content:SourceUrl>
		<content:ContentFileSize>15235</content:ContentFileSize>
		<content:ContentCheckSum>B3258D3269D25852BD26548DC2654CD2</content:ContentCheckSum>
		<content:PropagationPriority>1</content:PropagationPriority>
		<content:X_Resolution>320</content:X_Resolution>
		<content:Y_Resolution>240</content:Y_Resolution>
	</Asset>

	<Asset
		xsi:type="content:MovieType"
		uriId="indemand.com/Asset/UNVA2001081701004005"
		providerVersionNum="1"
		internalVersionNum="0"
		creationDateTime="2002-01-11T00:00:00Z"
		startDateTime="2002-02-01T00:00:00Z"
		endDateTime="2002-03-31T11:59:59Z"
	>
		<core:AlternateId identifierSystem="VOD1.1">vod://indemand.com/UNVA2001081701004005</core:AlternateId>
		<core:AssetName deprecated="true">The_Titanic_Encrypted.mpg</core:AssetName>
		<core:Product deprecated="true">First-Run</core:Product>
		<core:Provider>InDemand</core:Provider>
		<core:Description deprecated="true">The Titanic Movie Encrypted</core:Description>
		<core:Ext/>
		<core:MasterSourceRef uriId="indemand.com/Asset/UNVA2001081701004002"/>
		<content:SourceUrl>The_Titanic_Encrypted.mpg</content:SourceUrl>
		<content:ContentFileSize>3907840625</content:ContentFileSize>
		<content:ContentCheckSum>DC2654CA12558D3269D25852BD265482</content:ContentCheckSum>
		<content:PropagationPriority>1</content:PropagationPriority>
		<content:AudioType>Dolby Digital</content:AudioType>
		<content:ScreenFormat>Widescreen</content:ScreenFormat>
		<content:Duration>PT03H14M00S</content:Duration>
		<content:Language>en</content:Language>
		<content:SubtitleLanguage>es</content:SubtitleLanguage>
		<content:DubbedLanguage>es</content:DubbedLanguage>
		<content:Rating ratingSystem="MPAA">R</content:Rating>
		<content:EncryptionInfo>
			<content:ReceiverType>SomeReceiverType</content:ReceiverType>
		</content:EncryptionInfo>
	</Asset>
</ADI3>
```

### See Also

  * [SCTE-236 - Content Metadata Specification](https://scte-cms-resource-storage.s3.amazonaws.com/ANSI_SCTE-35-2019a-1582645390859.pdf)

