# Processing-ONIX-Feeds

## Where do ONIX files live?

Cory McCloud from Bibliometa has been collaborating with us to get ONIX records from approved partners into Open Library's catalog. As a start, ~50,000 [bookcovers](https://archive.org/details/onix-for-bookcovers) and [ONIX feed entries](https://archive.org/download/onix-for-books) have been added to archive.org items for an initial batch of processing lead by [Salman Shah](https://github.com/salman-bhai)

## What's an example of an ONIX entry?

```markup
 <Product>
  <RecordReference>0198520816</RecordReference>
  <NotificationType>03</NotificationType>
  <RecordSourceName>Cambridge University Press</RecordSourceName>
  <ProductIdentifier>
   <ProductIDType>02</ProductIDType>
   <IDValue>0198520816</IDValue>
  </ProductIdentifier>
  <ProductIdentifier>
   <ProductIDType>03</ProductIDType>
   <IDValue>9780198520818</IDValue>
  </ProductIdentifier>
  <ProductIdentifier>
   <ProductIDType>15</ProductIDType>
   <IDValue>9780198520818</IDValue>
  </ProductIdentifier>
  <Barcode>02</Barcode>
  <ProductForm>BC</ProductForm>
  <ProductFormDetail>B102</ProductFormDetail>
  <Series>
   <TitleOfSeries>New Surveys in the Classics</TitleOfSeries>
   <NumberWithinSeries>34</NumberWithinSeries>
  </Series>
  <Title>
   <TitleType>01</TitleType>
   <TitleText>Roman Art</TitleText>
  </Title>
  <Contributor>
   <SequenceNumber>1</SequenceNumber>
   <ContributorRole>A01</ContributorRole>
   <PersonName>Peter Stewart</PersonName>
   <PersonNameInverted>Stewart, Peter</PersonNameInverted>
   <NamesBeforeKey>Peter</NamesBeforeKey>
   <KeyNames>Stewart</KeyNames>
   <ProfessionalAffiliation>
    <Affiliation>Courtauld Institute of Art, London</Affiliation>
   </ProfessionalAffiliation>
  </Contributor>
  <NoEdition />
  <Language>
   <LanguageRole>01</LanguageRole>
   <LanguageCode>eng</LanguageCode>
  </Language>
  <BASICMainSubject>ART015060</BASICMainSubject>
  <BASICVersion>2.2</BASICVersion>
  <Subject>
   <SubjectSchemeIdentifier>12</SubjectSchemeIdentifier>
   <SubjectSchemeVersion>2.0</SubjectSchemeVersion>
   <SubjectCode>ACG</SubjectCode>
  </Subject>
  <Subject>
   <SubjectSchemeIdentifier>01</SubjectSchemeIdentifier>
   <SubjectSchemeVersion>22</SubjectSchemeVersion>
   <SubjectCode>709/.37</SubjectCode>
  </Subject>
  <Subject>
   <SubjectSchemeIdentifier>03</SubjectSchemeIdentifier>
   <SubjectCode>N5760 .S66 2004</SubjectCode>
  </Subject>
  <AudienceCode>05</AudienceCode>
  <MediaFile>
   <MediaFileTypeCode>04</MediaFileTypeCode>
   <MediaFileFormatCode>03</MediaFileFormatCode>
   <MediaFileLinkTypeCode>01</MediaFileLinkTypeCode>
   <MediaFileLink>http://assets.cambridge.org/97801985/20818/cover/9780198520818.jpg</MediaFileLink>
  </MediaFile>
  <Imprint>
   <NameCodeType>01</NameCodeType>
   <NameCodeTypeName>Cambridge University Press code</NameCodeTypeName>
   <NameCodeValue>OUP</NameCodeValue>
   <ImprintName>Oxford University Press</ImprintName>
  </Imprint>
  <Publisher>
   <PublishingRole>01</PublishingRole>
   <PublisherName>Oxford University Press</PublisherName>
  </Publisher>
  <CityOfPublication>Oxford</CityOfPublication>
  <CountryOfPublication>GB</CountryOfPublication>
  <PublishingStatus>04</PublishingStatus>
  <PublicationDate>20040422</PublicationDate>
  <SalesRights>
   <SalesRightsType>01</SalesRightsType>
   <RightsTerritory>ROW</RightsTerritory>
  </SalesRights>
  <SalesRights>
   <SalesRightsType>03</SalesRightsType>
   <RightsCountry>KP</RightsCountry>
  </SalesRights>
  <Measure>
   <MeasureTypeCode>01</MeasureTypeCode>
   <Measurement>9.25</Measurement>
   <MeasureUnitCode>in</MeasureUnitCode>
  </Measure>
  <Measure>
   <MeasureTypeCode>02</MeasureTypeCode>
   <Measurement>6.14</Measurement>
   <MeasureUnitCode>in</MeasureUnitCode>
  </Measure>
  <Measure>
   <MeasureTypeCode>03</MeasureTypeCode>
   <Measurement>.43</Measurement>
   <MeasureUnitCode>in</MeasureUnitCode>
  </Measure>
  <Measure>
   <MeasureTypeCode>08</MeasureTypeCode>
   <Measurement>.6</Measurement>
   <MeasureUnitCode>lb</MeasureUnitCode>
  </Measure>
  <SupplyDetail>
   <SupplierName>Cambridge University Press</SupplierName>
   <AvailabilityCode>IP</AvailabilityCode>
   <ProductAvailability>21</ProductAvailability>
   <PackQuantity>30</PackQuantity>
   <Price>
    <PriceTypeCode>01</PriceTypeCode>
    <BICDiscountGroupCode>ACUPRP  </BICDiscountGroupCode>
    <DiscountCoded>
     <DiscountCodeType>02</DiscountCodeType>
     <DiscountCode>P</DiscountCode>
    </DiscountCoded>
    <PriceStatus>02</PriceStatus>
    <PriceAmount>21.99</PriceAmount>
    <CurrencyCode>GBP</CurrencyCode>
    <TaxRateCode1>Z</TaxRateCode1>
    <TaxRatePercent1>0.00</TaxRatePercent1>
    <TaxableAmount1>21.99</TaxableAmount1>
    <TaxAmount1>0.00</TaxAmount1>
    <TaxRateCode2>Z</TaxRateCode2>
    <TaxRatePercent2>0.00</TaxRatePercent2>
    <TaxableAmount2>0.00</TaxableAmount2>
    <TaxAmount2>0.00</TaxAmount2>
   </Price>
   <Price>
    <PriceTypeCode>01</PriceTypeCode>
    <BICDiscountGroupCode>ACUPRP  </BICDiscountGroupCode>
    <DiscountCoded>
     <DiscountCodeType>02</DiscountCodeType>
     <DiscountCode>P</DiscountCode>
    </DiscountCoded>
    <PriceStatus>02</PriceStatus>
    <PriceAmount>25.66</PriceAmount>
    <CurrencyCode>EUR</CurrencyCode>
   </Price>
  </SupplyDetail>
  <SupplyDetail>
   <SupplierName>Cambridge University Press</SupplierName>
   <ReturnsCodeType>02</ReturnsCodeType>
   <ReturnsCode>C</ReturnsCode>
   <AvailabilityCode>IP</AvailabilityCode>
   <ProductAvailability>22</ProductAvailability>
   <PackQuantity>30</PackQuantity>
   <Price>
    <PriceTypeCode>01</PriceTypeCode>
    <ClassOfTrade>P</ClassOfTrade>
    <DiscountCoded>
     <DiscountCodeType>02</DiscountCodeType>
     <DiscountCode>P</DiscountCode>
    </DiscountCoded>
    <PriceStatus>02</PriceStatus>
    <PriceAmount>35.99</PriceAmount>
    <CurrencyCode>USD</CurrencyCode>
   </Price>
   <Price>
    <PriceTypeCode>01</PriceTypeCode>
    <ClassOfTrade>P</ClassOfTrade>
    <DiscountCoded>
     <DiscountCodeType>02</DiscountCodeType>
     <DiscountCode>P</DiscountCode>
    </DiscountCoded>
    <PriceStatus>02</PriceStatus>
    <PriceAmount>40.95</PriceAmount>
    <CurrencyCode>CAD</CurrencyCode>
   </Price>
  </SupplyDetail>
  <SupplyDetail>
   <SupplierName>Cambridge University Press</SupplierName>
   <ReturnsCodeType>02</ReturnsCodeType>
   <ReturnsCode>C</ReturnsCode>
   <AvailabilityCode>IP</AvailabilityCode>
   <ProductAvailability>22</ProductAvailability>
   <PackQuantity>30</PackQuantity>
   <Price>
    <PriceTypeCode>02</PriceTypeCode>
    <PriceStatus>02</PriceStatus>
    <PriceAmount>62.95</PriceAmount>
    <CurrencyCode>AUD</CurrencyCode>
   </Price>
   <Price>
    <PriceTypeCode>02</PriceTypeCode>
    <PriceStatus>02</PriceStatus>
    <PriceAmount>69.95</PriceAmount>
    <CurrencyCode>NZD</CurrencyCode>
   </Price>
  </SupplyDetail>
</Product>
```

## Which fields does Open Library care about?

### 1. TitleText = title

* **Full Path:** /Product/Title/TitleText
* **Accompanying Fields**: TitleType

| Title Type Code Value | Title Type Code Description |
| :--- | :--- |
| 00 | Undefined |
| 01 | Distinctive Title |
| 02 | ISSN Key Title of Serial |
| 03 | Title in Original Language |
| 04 | Title Acronymn |
| 05 | Abbreviated Title |
| 06 | Title in other language |
| 07 | Thematic title of journal issue |
| 08 | Former title |
| 10 | Distributor's title |

### 2. PublisherName = publisher

* **Full Path:** /Product/Publisher/PublisherName
* **Accompanying Fields**: PublishingRole

| Publishing Role Code Value | Publishing Role Code Description |
| :--- | :--- |
| 01 | Publisher |
| 02 | Co Publisher |
| 03 | Sponsor |
| 04 | Publisher of original language version |
| 05 | Host/distributor of electronic content |
| 06 | Published for/on behalf of |
| 07 | Published in association with |
| 08 | Published on behalf of |
| 09 | New or acquiring publisher |

### 3. CountryOfPublication = country where published  \(Great Britain\)

* **Full Path:** /Product/CountryOfPublication

### 4. CityOfPublication = city where published \(oxford\)-

* **Full Path:** /Product/CityOfPublication

### 5. MediaFileLink = bookcover

* **Full Path:** /Product/MediaFile/MediaFileLink
* **Accompanying Fields**: MediaFileTypeCode

| Media File Type Code Value | Media File Type Code Description |
| :--- | :--- |
| 01 | URL |
| 02 | DOI |
| 03 | PURL |
| 04 | URN |
| 05 | FTP Address |
| 06 | filename |

* **Accompanying Fields**: MediaFileFormatCode

| Media File Format Code Value | Media File Format Code Description |
| :--- | :--- |
| 02 | GIF |
| 03 | JPEG |
| 05 | TIF |

* **Accompanying Fields**: MediaFileLinkTypeCode

| Media File Link Type Code Value | Media File Link Type Code Description |
| :--- | :--- |
| 01 | URL |
| 02 | DOI |
| 03 | PURL |
| 04 | URN |
| 05 | FTP Address |
| 06 | filename |

* Language = Language \(there may be more than one\)
* Measure = the physical book dimensions which we can add

Some of the fields will need to be discussed:

1. Contributor: We _definitely_ want this data, it will be very valuable for creating new authors and merging authors. But, contributor could be author, editor \(could be a lot of things\). Question: Which means \# author? We need to ask @cory
2. Series: We'll want to ask \#openlibrary-g how we want to handle `series` \(the title and number\)
3. ProductIDTypes: We'll need to ask @cory for a list of ProductIDTypes \(e.g. which of these is ISBN?\) Suffice to say, a book may have multiple of these IDs and we'll want to add them
4. Subject we'll have to ask \(where/how to lookup the subject code\)

## Import Strategy

The import strategy to import these books is to have a bot which parses through the XML in the ONIX Record. The following steps are followed while processing the ONIX Feed: 1. Check if there are any errors in the XML File which contains the ONIX Records. 2. Parsing through a single ONIX Record which is under which is under the Product Tag. Inside a single Product Tag, we parse through tags like `ProductIdentifier`, `Title`, `Publisher`, `CountryOfPublication`, `CityOfPublication`, `MediaFile`, `Language` to obtain the required results. 3. 4.

