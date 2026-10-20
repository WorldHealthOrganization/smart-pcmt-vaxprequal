# PreQual Product: Nasovac-S Influenza Vaccine Live Attenuated (Human) - IIV3 - SMART Product Dataset for Prequalified Vaccines v0.2.0

* [**Table of Contents**](toc.md)
* [**Indices**](indices.md)
* [**Artifact Index**](artifacts.md)
* **PreQual Product: Nasovac-S Influenza Vaccine Live Attenuated (Human) - IIV3**

## Example Binary: PreQual Product: Nasovac-S Influenza Vaccine Live Attenuated (Human) - IIV3

This content is an example of the [WHO Vaccine PreQual DB - Finished Vaccine Products](StructureDefinition-FinishedVaccineProducts.md) Logical Model and is not a FHIR Resource

```

{
  "resourceType": "http://smart.who.int/pcmt-vaxprequal/StructureDefinition/FinishedVaccineProducts",
  "productType": "FinishedVaccineProduct",
  "dateOfPrequal": "2015-09-30",
  "assessmentProcedure": "PrequalificationStandard",
  "status": "Prequalified",
  "pharmaceuticalForm": "Lyophilisedactivecomponenttobereconstitutedwithexcipientdiluentbeforeuse",
  "presentation": {
    "coding": [
      {
        "system": "https://extranet.who.int/prequal/vaccines/prequalified-vaccines",
        "code": "VialAmpoule",
        "display": "Vial + Ampoule"
      }
    ]
  },
  "numDoses": 1,
  "productId": {
    "system": "https://extranet.who.int/prequal/api",
    "value": "a3K3X000006MijDUAS"
  },
  "productName": "FVP-P-284",
  "vaccineFullName": "Influenza vaccine trivalent (live, attenuated)",
  "vaccineAbbreviatedName": "IIV3",
  "vaccineCommercialName": "Nasovac-S Influenza Vaccine Live Attenuated (Human)",
  "vaccineTypeId": {
    "system": "https://extranet.who.int/prequal/api",
    "value": "a3S3X000003cSpPUAU"
  },
  "routeOfAdministration": "Intranasal",
  "vialMonitor": "Type 2",
  "applicantId": {
    "system": "https://extranet.who.int/prequal/api",
    "value": "0013X00003cPkzfQAC"
  },
  "applicantName": "Serum Institute of India",
  "nraId": {
    "system": "https://extranet.who.int/prequal/api",
    "value": "0013X0000498p4fQAA"
  },
  "nraName": "Central Drugs Standard Control Organization (CDSCO)",
  "nraCountry": "India",
  "storageTemperature": "2 - 8°C",
  "lastPublishingDate": "2024-09-10",
  "publishingRemarks": "Diluent: Sterile Water for InhalationShelf life: 9 monthsDiluent is cartoned separately and does not require 2-8°C storageThere are two sources of diluent with different packaging configurations:1) 17 cartons of 72 ampoules (1224 ampoules) [25.5 x 24.5  x 26.0 cm]2) 24 cartons of 50 ampoules (1200 ampoules) [29.4  x 29.0  x 28.8 cm]Additionally a shipper of 1200 nasal applicator kits are provided. (53.7 x 47.0 x 50.0 cm)",
  "manufacturerReference": {
    "reference": "Organization/Manufacturer0013X00003cPkzfQAC"
  },
  "responsibleNRAReference": {
    "reference": "Organization/Holder0013X0000498p4fQAA"
  },
  "manufacturerLM": {
    "reference": "http://smart.who.int/pcmt-vaxprequal/StructureDefinition/PreQualManufacturer/PreQualManufacturer0013X00003cPkzfQAC"
  },
  "nraLM": {
    "reference": "http://smart.who.int/pcmt-vaxprequal/StructureDefinition/PreQualNRA/PreQualNRA0013X0000498p4fQAA"
  },
  "vaccineLM": {
    "reference": "http://smart.who.int/pcmt-vaxprequal/StructureDefinition/PreQualVaccine/PreQualVaccinea3S3X000003cSpPUAU"
  },
  "packagingLM": [
    {
      "reference": "http://smart.who.int/pcmt-vaxprequal/StructureDefinition/PreQualProductPackaging/PreQualPackaginga3H3X000001VadQUAS"
    }
  ],
  "siteLM": [
    {
      "reference": "http://smart.who.int/pcmt-vaxprequal/StructureDefinition/PreQualSiteDetail/PreQualSite0013X00003cPkzfQAC"
    }
  ],
  "ingredientLM": [
    {
      "reference": "http://smart.who.int/pcmt-vaxprequal/StructureDefinition/PreQualProductIngredient/PreQualIngredienta3K3X000006MijDUASIng3"
    }
  ]
}

```



## Resource Binary Content

application/fhir+json:

```
{snip}
```
