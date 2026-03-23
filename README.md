# Belgian DCAT-AP (DCAT-BE) Web Form for Datasets

## 1. Purpose

This document describes the technical requirements to implement a Web form that complies with the **Federal DCAT-AP profile (DCAT-BE) for Belgium** to produce valid Dataset descriptions. 

Here we define the `mandatory`, `recommended` and `optional` fields that allow to collect dataset metadata according to the **DCAT-BE profile**. The resulting data must be serialized using the W3C JSON-LD standard, which must be suitable for harvesting by Belgian and EU data portals.

------

## 2. Applicable Specifications and Standards

- [Federal DCAT-AP Belgium (DCAT-BE)](https://github.com/belgif/inspire-dcat/blob/main/DCATAPprofil.en.md)
- [W3C DCAT v2](https://www.w3.org/TR/vocab-dcat-2/)*
- [Interoperable Europe DCAT-AP v2](https://interoperable-europe.ec.europa.eu/collection/semic-support-centre/solution/dcat-application-profile-data-portals-europe/release/200)*
- [W3C SKOS](https://www.w3.org/TR/skos-reference/)
- [W3C JSON-LD 1.1](https://www.w3.org/TR/json-ld11/)
- 

> *More recent versions of these standards currently exist, but the DCAT-BE specifications is limited to these older versions, which must be followed for compliance.   

#### Common prefixes:

| Prefix  | **Namespace IRI**                           |
| ------- | ------------------------------------------- |
| adms    | http://www.w3.org/ns/adms#                  |
| dcat    | http://www.w3.org/ns/dcat#                  |
| dcmi    | http://purl.org/dc/dcmitype/                |
| dcterms | http://purl.org/dc/terms/                   |
| dqv     | http://www.w3.org/ns/dqv#                   |
| foaf    | http://xmlns.com/foaf/0.1/                  |
| geodcat | http://data.europa.eu/930/                  |
| rdf     | http://www.w3.org/1999/02/22-rdf-syntax-ns# |
| sdmx-attribute | http://purl.org/linked-data/sdmx/2009/attribute# |
| skos    | http://www.w3.org/2004/02/skos/core#        |
| vcard   | http://www.w3.org/2006/vcard/ns#            |
| xsd     | http://www.w3.org/2001/XMLSchema#           |

------

## 3. General Requirements

The dataset form must produce *one* JSON-LD document per dataset with:

- Required dataset fields as per DCAT-BE
- Correct datatypes and cardinalities (must be enforced by the form)
- Multi-language support where applicable

DCAT-BE introduces *requirement levels* for attributes:

- **Mandatory (M)** — must be present
- **Recommended (R)** — should be provided if available
- **Optional (O)** — may be provided

The Web form should at minimum collect all **Mandatory** properties.

------

## 4. `dcat:Dataset` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for creating a DCAT Dataset description.

| Field                                | JSON-LD Property             | Range Type                                         | Cardinality | DCAT-BE Level | Predefined Value(s)                                          |
| ------------------------------------ | ---------------------------- | -------------------------------------------------- | ----------- | ------------- | ------------------------------------------------------------ |
| [Dataset IRI](#411-dataset-iri)    | `@id`                        | IRI                                                | 1           | M             |                                                              |
| [Type](#412-type)                  | `@type`                      | IRI                                                | 1           | M             | `dcat:Dataset`                                               |
| [Title](#413-title)                | `dcterms:title`              | `rdf:langString`                                   | 1           | M             |                                                              |
| [Description](#414-description)    | `dcterms:description`        | `rdf:langString`                                   | 1           | M             |                                                              |
| [Identifier](#415-identifier)      | `dcterms:identifier`         | `xsd:string`                                       | 1           | M             |                                                              |
| [Access Rights](#416-access-rights)                        | `dcterms:accessRights`       | IRI                                                | 1           | M             | [see INSPIRE controlled vocabulary](https://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess) |
| [License](#417-license)                              | `dcterms:license`            | [`dcterms:LicenseDocument`](#6-dctermsLicenseDocument-fields) | 1           | M             |                                                              |
| [Theme](#418-theme)                                | `dcat:theme`                 | IRI                                                | 1...n       | M             | [see Belgif vocabulary](https://vocab.belgif.be/auth/datatheme/) |
| [Distribution](#419-distribution)                         | `dcat:distribution`          | [`dcat:Distribution`](#5-dcatdistribution-fields) | 0...n       | R             |                                                              |
| [Publisher](#4.1.10-publisher) | `dcterms:publisher` | [`foaf:Organization`](#11-foaforganization-fields) | 0...n | R | |
| [Modified Date](#4.1.11-modified-date) | `dcterms:modified` | `xsd:date` | 0...1 | R | |
| [Created Date](#4.1.12-created-date) | `dcterms:created` | `xsd:date` | 0...1 | R | |
| [Spatial Coverage](#4.1.13-spatial-coverage) | `dcterms:spatial` | [`dcterms:Location`](#7-dctermslocation-fields) | 0...n | R | |
| [Temporal Coverage](#4.1.14-temporal-coverage) | `dcterms:temporal` | [`dcterms:PeriodOfTime`](#8-dctermsperiodoftime-fields) | 0...n | R | |
| [Landing Page](#4.1.15-landing-page) | `dcat:landingPage` | IRI | 0...4 | R | https://favv-afsca.be/nl/open-data |
| [Metadata Page](#4.1.16-metadata-page) | `foaf:page` | IRI | 0...1 | R | |
| [Update Frequency](#4.1.17-update-frequency) | `dcterms:accrualPeriodicity` | IRI | 0...1 | R | |
| [Language](#4.1.18-language) | `dcterms:language` | IRI | 0...4 | R | [See EU Language code list](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/language) |
| [Spatial Resolution](#4.1.19-spatial-resolution) | `dqv:hasQualityMeasurement` | [`dqv:QualityMeasurement`](#9-dqvqualitymeasurement-fields) | 0...1 | R | |
| [Type Dataset](#4.1.20-type-dataset) | `adms:representationTechnique` | IRI | 1 | R | |
| [Contact Point](#4.1.21-contact-point) | `dcat:contactPoint` | [`vcard:Organization`](#10-vcardorganization-fields) | 0...n | O | |
| [Issued Date](#4.1.22-issued-date) | `dcterms:issued` | `xsd:date` | 0...1 | O | |
| [Keyword](#4123-keyword)                              | `dcat:keyword`               | `rdf:langString`                                   | 0...n       | O             |                                                              |
| [Subject](#4124-subject)                              | `dcterms:subject`            | IRI                                                | 0...n       | O             |                                                              |
| [Origin](#4125-origin)                               | `dcterms:provenance`         | `dcterms:ProvenanceStatement`                      | 0...1       | O             |                                                              |
| [Specification or Rule of Structuring](#4126-specification-or-rule-of-structuring) | `dcterms:conformsTo`         | `dcterms:Standard`                                 | 0...n       | O             |                                                              |
| [Source Dataset](#4127-source-dataset)                       | `dcterms:source`             | `dcat:Dataset`                                     | 0...1       | O             |                                                              |
| [Distribution Example](#4128-distribution-example)                 | `adms:sample`                | [`dcat:Distribution`](#5-dcatdistribution-fields) | 0...1       | O             |                                                              |
| [Maintainer](#4129-maintainer-and-other-agent-roles)                           | `geodcat:custodian`          | [`foaf:Organization`](#11-foaforganization-fields) | 0...n       | O             |                                                              |
| [Creator](#4129-maintainer-and-other-agent-roles)                              | `dcterms:creator`            | [`foaf:Organization`](#11-foaforganization-fields) | 0...n       | O             |                                                              |
| [Diffuser](#4129-maintainer-and-other-agent-roles)                             | `geodcat:distributor`        | [`foaf:Organization`](#11-foaforganization-fields) | 0...n       | O             |                                                              |
| [Author](#4129-maintainer-and-other-agent-roles)                               | `geodcat:originator`         | [`foaf:Organization`](#11-foaforganization-fields) | 0...n       | O             |                                                              |
| [Collector](#4129-maintainer-and-other-agent-roles)                            | `geodcat:principalInvestigator` | [`foaf:Organization`](#11-foaforganization-fields) | 0...n       | O             |                                                              |
| [Processor](#4129-maintainer-and-other-agent-roles)                            | `geodcat:processor`          | [`foaf:Organization`](#11-foaforganization-fields) | 0...n       | O             |                                                              |
| [Provider](#4129-maintainer-and-other-agent-roles)                             | `geodcat:resourceProvider`   | [`foaf:Organization`](#11-foaforganization-fields) | 0...n       | O             |                                                              |
| [User](#4129-maintainer-and-other-agent-roles)                                 | `geodcat:user`               | [`foaf:Organization`](#11-foaforganization-fields) | 0...n       | O             |                                                              |
| [Rights Holder](#4130-rights-holder)                        | `dcterms:rightsHolder`       | [`foaf:Organization`](#11-foaforganization-fields) | 0...1       | O             |                                                              |

------

### 4.1 Field-by-Field Specifications

#### 4.1.1 Dataset IRI

- **Form element**: text input

- **Description**: Global Web identifier of the dataset.

- **cardinality**: 1

- **Datatype**: IRI

- **Mapped to**: `@id`

- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.

- **Example**:

  ```json
  {
      "@id": "https://favv-afsca.be/nl/open-data/inter_liste_smiley"
  }
  ```

#### 4.1.2 Type

* **Form element**: text input

* **Description**: Type identifier, according to the DCAT standard.

* **cardinality**: 1

* **Datatype**: IRI

* **Mapped to**: `@type`

* **Predefined value**: `dcat:Dataset`

* **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.

* **Example**:

  ```json
  {
      "@type": "dcat:Dataset"
  }
  ```

#### 4.1.3 Title

* **Form element**: text input (per language)

* **Description**: Title of the dataset considered.

* **Cardinality**: 1

* **Datatype**: `rdf:langString`

* **Mapped to**: `dcterms:title`

* **Multilingual**: `nl`, `fr`, `de`, `en`

* **Example**:

  ```json
  {
    "dcterms:title": [
      { "@value": "titel in het nederlands", "@language": "nl" },
      { "@value": "titre en français", "@language": "fr" },
      { "@value": "titel auf deutsch", "@language": "de" },
      { "@value": "title in english", "@language": "en" }
    ]
  }
  ```

#### 4.1.4 Description

* **Form element**: text input (per language)

* **Description**: Description of the dataset considered.

* **Cardinality**: 1

* **Datatype**: `rdf:langString`

* **Mapped to**: `dcterms:description`

* **Multilingual**: `nl`, `fr`, `de`, `en`

* **Example**:

  ```json
  {
      "dcterms:description": [
          { "value": "beschrijving in het nederlands", "@language": "nl"},
          { "value": "description en français", "@language": "fr"},
          { "value": "beschreibung auf deutsch", "@language": "de"},
          { "value": "description in english", "@language": "en"},
      ]
  }
  ```

#### 4.1.5 Identifier

- **Form element**: text input

- **Description**: Alphanumeric identifier unique to the considered dataset

- **Cardinality**: 1

- **Datatype**: `xsd:string`

- **Mapped to**: `dcterms:identifier`

  ```json
  {
      "dcterms:identifier": "list-smilies"
  }
  ```

#### 4.1.6 Access Rights

- **Form element**: select list (controlled vocabulary)
- **Description**: Information about who can access the dataset or under what conditions.
- **Cardinality**: 1
- **Datatype**: IRI
- **Mapped to**: `dcterms:accessRights`
- **Predefined values**: [INSPIRE registry for Limitations on Public Access](https://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  
  ```json
  {
     "dcterms:accessRights": { 
        "@id": "http://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess/noLimitations" 
     }
  }
  ```

#### 4.1.7 License

- **Form element**: [See section 6](#6-dctermsLicenseDocument-fields) for specification of `dcterms:LicenseDocument`
- **Description**: The license under which the dataset is made available.
- **Cardinality**: 1
- **Datatype**: IRI
- **Mapped to**: `dcterms:license`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:license": {
        "@type": "dcterms:LicenseDocument",
        "dcterms:title": [
            {
                "@value": "Licentietitel",
                "@language": "nl"
            }
        ],
        "dcterms:type": "dcmi:Text",
        "dcterms:description": {
            "@language": "nl",
            "@value": "Open data zijn openbare, niet persoonsgebonden gegevens die in eenmachinaal leesbaar formaat worden aangeboden en gratis te hergebruiken zijn, zowel voor commercieel als niet-commercieel gebruik, op voorwaarde dat de gebruiker de bron en de datum van laatste bijwerking vermeldt. De inhoud van de hergebruikte informatie mag niet misleidend zijn. In het bijzonder mag deze inhoud geen aanleiding geven tot de veronderstelling dat de gebruiker verbonden is met, de steun heeft van, goedgekeurd is door of een officieel statuut heeft verkregen van het FAVV"
        }
    }
  }
  ```

#### 4.1.8 Theme

- **Form element**: multi-select (controlled vocabulary)
- **Description**: The main category or topic of the dataset.
- **Cardinality**: 1...n
- **Datatype**: IRI
- **Mapped to**: `dcat:theme`
- **Predefined values**: [Data Theme Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/data-theme)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcat:theme": [
      { "@id": "http://publications.europa.eu/resource/authority/data-theme/AGRI" },
      { "@id": "http://publications.europa.eu/resource/authority/data-theme/ENVI" }
    ]
  }
  ```

#### 4.1.9 Distribution

- **Form element**: sub-form (repeatable)
- **Description**: A physical embodiment of the dataset (e.g., a CSV file, an API).
- **Cardinality**: 0...n
- **Range Type**: `dcat:Distribution` (see Section 5)
- **Mapped to**: `dcat:distribution`
- **Example**:
  ```json
  {
    "dcat:distribution": [
      { "@id": "https://favv-afsca.be/nl/open-data/distribution/list-smilies-csv" }
    ]
  }
  ```

#### 4.1.10 Publisher

- **Form element**: text input (IRI) or organization lookup
- **Description**: The entity (organization) responsible for making the dataset available.
- **Cardinality**: 0...n
- **Datatype**: IRI / `foaf:Organization`
- **Mapped to**: `dcterms:publisher`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:publisher": { 
      "@id": "https://org.belgium.be/favv-afsca",
      "@type": "foaf:Organization"
    }
  }
  ```

#### 4.1.11 Modified Date

- **Form element**: date picker
- **Description**: Most recent date on which the dataset was changed or modified.
- **Cardinality**: 0...1
- **Datatype**: `xsd:date`
- **Mapped to**: `dcterms:modified`
- **Example**:
  ```json
  {
    "dcterms:modified": { "@value": "2025-02-22", "@type": "xsd:date" }
  }
  ```

#### 4.1.12 Created Date

- **Form element**: date picker
- **Description**: Date of formal creation of the dataset.
- **Cardinality**: 0...1
- **Datatype**: `xsd:date`
- **Mapped to**: `dcterms:created`
- **Example**:
  ```json
  {
    "dcterms:created": { "@value": "2024-12-01", "@type": "xsd:date" }
  }
  ```

#### 4.1.13 Spatial Coverage

- **Form element**: text input (IRI) or geographic selector
- **Description**: The geographic area covered by the dataset.
- **Cardinality**: 0...n
- **Range Type**: `dcterms:Location`
- **Mapped to**: `dcterms:spatial`
- **Example**:
  ```json
  {
    "dcterms:spatial": { "@id": "http://publications.europa.eu/resource/authority/country/BEL" }
  }
  ```

#### 4.1.14 Temporal Coverage

- **Form element**: date range picker
- **Description**: The period of time covered by the dataset.
- **Cardinality**: 0...n
- **Range Type**: `dcterms:PeriodOfTime`
- **Mapped to**: `dcterms:temporal`
- **Example**:
  ```json
  {
    "dcterms:temporal": {
      "@type": "dcterms:PeriodOfTime",
      "dcat:startDate": { "@value": "2024-01-01", "@type": "xsd:date" },
      "dcat:endDate": { "@value": "2024-12-31", "@type": "xsd:date" }
    }
  }
  ```

#### 4.1.15 Landing Page

- **Form element**: text input (URL)
- **Description**: A Web page that can be navigated to in a Web browser to gain access to the dataset, its distributions and/or additional information.
- **Cardinality**: 0...4
- **Datatype**: IRI
- **Mapped to**: `dcat:landingPage`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcat:landingPage": { "@id": "https://favv-afsca.be/nl/open-data/smiley-project" }
  }
  ```

#### 4.1.16 Metadata Page

- **Form element**: text input (URL)
- **Description**: A page that provides information about the dataset.
- **Cardinality**: 0...1
- **Datatype**: IRI
- **Mapped to**: `foaf:page`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "foaf:page": { "@id": "https://favv-afsca.be/nl/open-data/smiley-metadata.html" }
  }
  ```

#### 4.1.17 Update Frequency

- **Form element**: select list (controlled vocabulary)
- **Description**: The frequency at which the dataset is updated.
- **Cardinality**: 0...1
- **Datatype**: IRI
- **Mapped to**: `dcterms:accrualPeriodicity`
- **Predefined values**: [Frequency Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/frequency)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:accrualPeriodicity": { "@id": "http://publications.europa.eu/resource/authority/frequency/MONTHLY" }
  }
  ```

#### 4.1.18 Language

- **Form element**: multi-select (controlled vocabulary)
- **Description**: The language(s) of the dataset.
- **Cardinality**: 0...4
- **Datatype**: IRI
- **Mapped to**: `dcterms:language`
- **Predefined values**: [Language Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/language)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  
  ```json
  {
    "dcterms:language": [
      { "@id": "http://publications.europa.eu/resource/authority/language/NLD" },
      { "@id": "http://publications.europa.eu/resource/authority/language/FRA" },
      { "@id": "http://publications.europa.eu/resource/authority/language/DEU" },
      { "@id": "http://publications.europa.eu/resource/authority/language/ENG" }
    ]
  }
  ```

#### 4.1.19 Spatial Resolution

- **Form element**: numeric input or distance
- **Description**: The minimum distance between two adjacent objects that can be distinguished.
- **Cardinality**: 0...1
- **Range Type**: `dqv:QualityMeasurement`
- **Mapped to**: `dqv:hasQualityMeasurement`
- **Example**:
  ```json
  {
    "dqv:hasQualityMeasurement": {
      "@type": "dqv:QualityMeasurement",
      "dqv:isMeasurementOf": { "@id": "http://data.europa.eu/dr8/SpatialResolution" },
      "dqv:value": { "@value": "10.0", "@type": "xsd:decimal" },
      "sdmx-attribute:unitMeasure": { "@id": "http://publications.europa.eu/resource/authority/unit/M" }
    }
  }
  ```

#### 4.1.20 Type Dataset

- **Form element**: select list (controlled vocabulary)
- **Description**: The technique used to represent the data.
- **Cardinality**: 1
- **Datatype**: IRI
- **Mapped to**: `adms:representationTechnique`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "adms:representationTechnique": { "@id": "http://publications.europa.eu/resource/authority/representation-technique/VECTOR" }
  }
  ```

#### 4.1.21 Contact Point

- **Form element**: sub-form
- **Description**: Contact information that can be used for sending comments about the dataset.
- **Cardinality**: 0...n
- **Range Type**: `vcard:Organization`
- **Mapped to**: `dcat:contactPoint`
- **Example**:
  ```json
  {
    "dcat:contactPoint": {
      "@type": "vcard:Organization",
      "vcard:fn": "FAVV Open Data Team",
      "vcard:hasEmail": { "@id": "mailto:opendata@favv.be" }
    }
  }
  ```

#### 4.1.22 Issued Date

- **Form element**: date picker
- **Description**: Date of formal issuance (e.g., publication) of the dataset.
- **Cardinality**: 0...1
- **Datatype**: `xsd:date`
- **Mapped to**: `dcterms:issued`
- **Example**:
  ```json
  {
    "dcterms:issued": { "@value": "2025-01-15", "@type": "xsd:date" }
  }
  ```

#### 4.1.23 Keyword

- **Form element**: repeatable text input (per language)
- **Description**: A keyword or term describing the dataset.
- **Cardinality**: 0...n
- **Datatype**: `rdf:langString`
- **Mapped to**: `dcat:keyword`
- **Example**:
  ```json
  {
    "dcat:keyword": [
      { "@value": "voedselveiligheid", "@language": "nl" },
      { "@value": "sécurité alimentaire", "@language": "fr" }
    ]
  }
  ```

#### 4.1.24 Subject

- **Form element**: multi-select (controlled vocabulary)
- **Description**: The topic of the dataset according to a controlled vocabulary.
- **Cardinality**: 0...n
- **Datatype**: IRI
- **Mapped to**: `dcterms:subject`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:subject": { "@id": "http://eurovoc.europa.eu/2735" }
  }
  ```

#### 4.1.25 Origin

- **Form element**: text area
- **Description**: Information about the origin or lineage of the dataset.
- **Cardinality**: 0...1
- **Range Type**: `dcterms:ProvenanceStatement`
- **Mapped to**: `dcterms:provenance`
- **Example**:
  ```json
  {
    "dcterms:provenance": {
      "@type": "dcterms:ProvenanceStatement",
      "dcterms:description": { "@value": "Gegevens afkomstig uit de Smiley-databank van de FAVV.", "@language": "nl" }
    }
  }
  ```

#### 4.1.26 Specification or Rule of Structuring

- **Form element**: repeatable text input (IRI)
- **Description**: An established standard or specification to which the dataset conforms.
- **Cardinality**: 0...n
- **Range Type**: `dcterms:Standard`
- **Mapped to**: `dcterms:conformsTo`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:conformsTo": { "@id": "https://www.w3.org/TR/vocab-dcat-2/" }
  }
  ```

#### 4.1.27 Source Dataset

- **Form element**: text input (IRI)
- **Description**: A dataset from which the current dataset is derived.
- **Cardinality**: 0...1
- **Range Type**: `dcat:Dataset`
- **Mapped to**: `dcterms:source`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:source": { "@id": "https://data.belgium.be/dataset/original-food-safety-data" }
  }
  ```

#### 4.1.28 Distribution Example

- **Form element**: sub-form
- **Description**: A sample distribution of the dataset.
- **Cardinality**: 0...1
- **Range Type**: `dcat:Distribution`
- **Mapped to**: `adms:sample`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "adms:sample": { "@id": "https://favv-afsca.be/nl/open-data/sample/smiley-sample.csv" }
  }
  ```

#### 4.1.29 Maintainer (and other Agent Roles)

- **Form element**: text input (IRI) or organization lookup
- **Mapped to**: `geodcat:custodian` (Maintainer), `dcterms:creator` (Creator), `geodcat:distributor` (Diffuser), `geodcat:originator` (Author), `geodcat:principalInvestigator` (Collector), `geodcat:processor` (Processor), `geodcat:resourceProvider` (Provider), `geodcat:user` (User).
- **Description**: Various organizational roles responsible for the dataset.
- **Cardinality**: 0...n
- **Datatype**: `foaf:Organization`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "geodcat:custodian": { 
      "@id": "https://org.belgium.be/favv-afsca-it-support",
      "@type": "foaf:Organization"
    },
    "dcterms:creator": {
      "@id": "https://org.belgium.be/favv-inspectorate",
      "@type": "foaf:Organization"
    }
  }
  ```

#### 4.1.30 Rights Holder

- **Form element**: text input (IRI) or organization lookup
- **Description**: The entity that holds the rights to the dataset.
- **Cardinality**: 0...1
- **Datatype**: `foaf:Organization`
- **Mapped to**: `dcterms:rightsHolder`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:rightsHolder": { "@id": "https://org.belgium.be/belgian-state" }
  }
  ```

------


## 5. `dcat:Distribution` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for creating a DCAT Distribution description.

| Field | JSON-LD Property | Range Type | Cardinality | DCAT-BE Level | Predefined Value(s) |
|---|---|---|---|---|---|
| [Access URL](#511-access-url) | `dcat:accessURL` | IRI | 1 | M | |
| [Format](#512-format) | `dcterms:format` | IRI | 1 | M | [File Type Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/file-type) |
| [Title](#513-title) | `dcterms:title` | `rdf:langString` | 0...1 | O | |
| [Description](#514-description) | `dcterms:description` | `rdf:langString` | 0...1 | O | |
| [Media Type](#515-media-type) | `dcat:mediaType` | IRI | 0...1 | O | [IANA Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml) |
| [Download URL](#516-download-url) | `dcat:downloadURL` | IRI | 0...1 | O | |
| [Compression Format](#517-compression-format) | `dcat:compressFormat` | IRI | 0...1 | O | |
| [Conforms To](#518-conforms-to) | `dcterms:conformsTo` | `dcterms:Standard` | 0...1 | O | |
| [Byte Size](#519-byte-size) | `dcat:byteSize` | `xsd:decimal` | 0...1 | O | |
| [Status](#5110-status) | `adms:status` | IRI | 0...1 | O | [ADMS Status Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/dataset-status) |
| [Spatial Coverage](#5111-spatial-coverage) | `dcterms:spatial` | `dcterms:Location` | 0...1 | O | |
| [Temporal Coverage](#5112-temporal-coverage) | `dcterms:temporal` | `dcterms:PeriodOfTime` | 0...1 | R | |
| [Type](#5113-type) | `dcterms:type` | IRI | 0...n | O | [Distribution Type Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/distribution-type) |
| [Language](#5114-language) | `dcterms:language` | IRI | 0...4 | R | [Language Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/language) |

------

### 5.1 Field-by-field specification

#### 5.1.1 Access URL

- **Form element**: text input (URL)
- **Description**: A URL that gives access to a distribution of the dataset (e.g., landing page, folder, or direct link).
- **Cardinality**: 1
- **Datatype**: IRI
- **Mapped to**: `dcat:accessURL`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcat:accessURL": { "@id": "https://favv-afsca.be/nl/open-data/smiley-list-2024" }
  }
  ```

#### 5.1.2 Format

- **Form element**: select list (controlled vocabulary)
- **Description**: The file format of the distribution.
- **Cardinality**: 1
- **Datatype**: IRI
- **Mapped to**: `dcterms:format`
- **Predefined values**: [File Type Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/file-type)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:format": { "@id": "http://publications.europa.eu/resource/authority/file-type/CSV" }
  }
  ```

#### 5.1.3 Title

- **Form element**: text input (per language)
- **Description**: A name given to the distribution.
- **Cardinality**: 0...1
- **Datatype**: `rdf:langString`
- **Mapped to**: `dcterms:title`
- **Example**:
  ```json
  {
    "dcterms:title": { "@value": "Smiley list 2024 (CSV)", "@language": "en" }
  }
  ```

#### 5.1.4 Description

- **Form element**: text area (per language)
- **Description**: A free-text account of the distribution.
- **Cardinality**: 0...1
- **Datatype**: `rdf:langString`
- **Mapped to**: `dcterms:description`
- **Example**:
  ```json
  {
    "dcterms:description": { "@value": "Detailed list of companies with a Smiley in 2024.", "@language": "en" }
  }
  ```

#### 5.1.5 Media Type

- **Form element**: text input or select (IANA media types)
- **Description**: The media type (MIME type) of the distribution.
- **Cardinality**: 0...1
- **Datatype**: IRI
- **Mapped to**: `dcat:mediaType`
- **Predefined values**: [IANA Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcat:mediaType": { "@id": "https://www.iana.org/assignments/media-types/text/csv" }
  }
  ```

#### 5.1.6 Download URL

- **Form element**: text input (URL)
- **Description**: A direct link to a downloadable file in a given format.
- **Cardinality**: 0...1
- **Datatype**: IRI
- **Mapped to**: `dcat:downloadURL`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcat:downloadURL": { "@id": "https://favv-afsca.be/download/smiley-2024.csv" }
  }
  ```

#### 5.1.7 Compression Format

- **Form element**: text input (IRI)
- **Description**: The compression format of the distribution (e.g., ZIP, GZIP).
- **Cardinality**: 0...1
- **Datatype**: IRI
- **Mapped to**: `dcat:compressFormat`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcat:compressFormat": { "@id": "http://www.iana.org/assignments/media-types/application/zip" }
  }
  ```

#### 5.1.8 Conforms To

- **Form element**: text input (IRI)
- **Description**: An established standard or specification to which the distribution conforms.
- **Cardinality**: 0...1
- **Range Type**: `dcterms:Standard`
- **Mapped to**: `dcterms:conformsTo`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:conformsTo": { "@id": "http://www.opengis.net/def/crs/EPSG/0/4326" }
  }
  ```

#### 5.1.9 Byte Size

- **Form element**: number input
- **Description**: The size of a distribution in bytes.
- **Cardinality**: 0...1
- **Datatype**: `xsd:decimal`
- **Mapped to**: `dcat:byteSize`
- **Example**:
  ```json
  {
    "dcat:byteSize": { "@value": "1048576", "@type": "xsd:decimal" }
  }
  ```

#### 5.1.10 Status

- **Form element**: select list (controlled vocabulary)
- **Description**: The maturity level of the distribution.
- **Cardinality**: 0...1
- **Datatype**: IRI
- **Mapped to**: `adms:status`
- **Predefined values**: [ADMS Status Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/dataset-status)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "adms:status": { "@id": "http://publications.europa.eu/resource/authority/dataset-status/COMPLETED" }
  }
  ```

#### 5.1.11 Spatial Coverage

- **Form element**: geographic selector or IRI
- **Description**: The geographic area covered by the distribution.
- **Cardinality**: 0...1
- **Range Type**: `dcterms:Location`
- **Mapped to**: `dcterms:spatial`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:spatial": { "@id": "http://publications.europa.eu/resource/authority/country/BEL" }
  }
  ```

#### 5.1.12 Temporal Coverage

- **Form element**: date range picker
- **Description**: The period of time covered by the distribution.
- **Cardinality**: 0...1
- **Range Type**: `dcterms:PeriodOfTime`
- **Mapped to**: `dcterms:temporal`
- **Example**:
  ```json
  {
    "dcterms:temporal": {
      "@type": "dcterms:PeriodOfTime",
      "dcat:startDate": { "@value": "2024-01-01", "@type": "xsd:date" },
      "dcat:endDate": { "@value": "2024-12-31", "@type": "xsd:date" }
    }
  }
  ```

#### 5.1.13 Type

- **Form element**: select list (controlled vocabulary)
- **Description**: The type of the distribution.
- **Cardinality**: 0...n
- **Datatype**: IRI
- **Mapped to**: `dcterms:type`
- **Predefined values**: [Distribution Type Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/distribution-type)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:type": { "@id": "http://publications.europa.eu/resource/authority/distribution-type/DOWNLOADABLE_FILE" }
  }
  ```

#### 5.1.14 Language

- **Form element**: multi-select (controlled vocabulary)
- **Description**: The language(s) of the distribution.
- **Cardinality**: 0...4
- **Datatype**: IRI
- **Mapped to**: `dcterms:language`
- **Predefined values**: [Language Codelist](https://publications.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/authority/language)
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:language": { "@id": "http://publications.europa.eu/resource/authority/language/NLD" }
  }
  ```


## 6. `dcterms:LicenseDocument` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for defining License Document descriptions.

| Field | JSON-LD Property | Range Type | Cardinality | DCAT-BE Level | Predefined Value(s) |
|---|---|---|---|---|---|
| [Title](#611-title) | `dcterms:title` | `rdf:langString` | 1...n | M | `dcmi:Text` |
| [Type](#612-type) | `dcterms:type` | IRI | 1 | R |  |
| [Description](#613-description) | `dcterms:description` | `rdf:langString` | 0...1 | O | |

------

### 6.1 Field-by-field specification

#### 6.1.1 Title

- **Form element**: text input (per language)
- **Description**: A name given to the license document.
- **Cardinality**: 1...n
- **Datatype**: `rdf:langString`
- **Mapped to**: `dcterms:title`
- **Example**:
  ```json
  {
    "dcterms:title": [
      { "@value": "Licentietitel", "@language": "nl" },
      { "@value": "Titre de la licence", "@language": "fr" }
    ]
  }
  ```

#### 6.1.2 Type

- **Form element**: text input (IRI)

- **Description**: The nature or genre of the document.

- **Cardinality**: 1

- **Datatype**: IRI

- **Mapped to**: `dcterms:type`

- **Predefined values**: `dcmi:Text`

- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.

- **Example**:
  
  ```json
  {
    "dcterms:type": { "@id": "http://purl.org/dc/dcmitype/Text" }
  }
  ```

#### 6.1.3 Description

- **Form element**: text area (per language)
- **Description**: A brief description of the license.
- **Cardinality**: 0...1
- **Datatype**: `rdf:langString`
- **Mapped to**: `dcterms:description`
- **Example**:
  ```json
  {
    "dcterms:description": {
      "@language": "nl",
      "@value": "Open data zijn openbare, niet persoonsgebonden gegevens..."
    }
  }
  ```

------

## 7. `dcterms:Location` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for defining Location descriptions.

| Field | JSON-LD Property | Range Type | Cardinality | DCAT-BE Level | Predefined Value(s) |
|---|---|---|---|---|---|
| [Geometry](#711-geometry) | `locn:geometry` | `xsd:string` | 1...4 | M | WKT or GML |
| [Geographic Bounding Box](#712-geographic-bounding-box) | `dcat:bbox` | `xsd:string` | 1...4 | M | |
| [Name](#713-name) | `skos:prefLabel` | `rdf:langString` | 0...1 | R | |
| [Identifier](#714-identifier) | `dcterms:identifier` | IRI | 0...1 | R | |

------

### 7.1 Field-by-field specification

#### 7.1.1 Geometry

- **Form element**: text area (WKT) or map geometry picker
- **Description**: The geometry of the location (e.g., in WKT format).
- **Cardinality**: 1...4
- **Datatype**: `xsd:string`
- **Mapped to**: `locn:geometry`
- **Example**:
  ```json
  {
    "locn:geometry": { "@value": "POLYGON((...))", "@type": "geosparql:wktLiteral" }
  }
  ```

#### 7.1.2 Geographic Bounding Box

- **Form element**: text input
- **Description**: The geographic bounding box of the location.
- **Cardinality**: 1...4
- **Datatype**: `xsd:string`
- **Mapped to**: `dcat:bbox`
- **Example**:
  ```json
  {
    "dcat:bbox": { "@value": "POLYGON((...))", "@type": "geosparql:wktLiteral" }
  }
  ```

#### 7.1.3 Name

- **Form element**: text input (per language)
- **Description**: The name of the geographical entity.
- **Cardinality**: 0...1
- **Datatype**: `rdf:langString`
- **Mapped to**: `skos:prefLabel`
- **Example**:
  ```json
  {
    "skos:prefLabel": { "@value": "België", "@language": "nl" }
  }
  ```

#### 7.1.4 Identifier

- **Form element**: text input (IRI or code)
- **Description**: An identifier for the geographic entity.
- **Cardinality**: 0...1
- **Datatype**: IRI
- **Mapped to**: `dcterms:identifier`
- **Notes**: The form must validate that a well-formed IRI is given according [RFC3987](https://www.ietf.org/rfc/rfc3987.txt). Ideally it should be a dereference-able IRI.
- **Example**:
  ```json
  {
    "dcterms:identifier": { "@id": "http://publications.europa.eu/resource/authority/country/BEL" }
  }
  ```

------

## 8. `dcterms:PeriodOfTime` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for defining Period of Time descriptions.

| Field | JSON-LD Property | Range Type | Cardinality | DCAT-BE Level | Predefined Value(s) |
|---|---|---|---|---|---|
| [Start Date](#811-start-date) | `dcat:startDate` | `xsd:date` / `xsd:dateTime` | 1 | M | |
| [End Date](#812-end-date) | `dcat:endDate` | `xsd:date` / `xsd:dateTime` | 1 | M | |

------

### 8.1 Field-by-field specification

#### 8.1.1 Start Date

- **Form element**: date/time picker
- **Description**: The start of the period.
- **Cardinality**: 1
- **Datatype**: `xsd:date` or `xsd:dateTime`
- **Mapped to**: `dcat:startDate`
- **Example**:
  ```json
  {
    "dcat:startDate": { "@value": "2024-01-01", "@type": "xsd:date" }
  }
  ```

#### 8.1.2 End Date

- **Form element**: date/time picker
- **Description**: The end of the period.
- **Cardinality**: 1
- **Datatype**: `xsd:date` or `xsd:dateTime`
- **Mapped to**: `dcat:endDate`
- **Example**:
  ```json
  {
    "dcat:endDate": { "@value": "2024-12-31", "@type": "xsd:date" }
  }
  ```

------

## 9. `dqv:QualityMeasurement` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for defining Quality Measurement (specifically for Spatial Resolution).

| Field | JSON-LD Property | Range Type | Cardinality | DCAT-BE Level | Predefined Value(s) |
|---|---|---|---|---|---|
| [Measured Scalar Quantity](#9.1.1-measured-scalar-quantity) | `dqv:isMeasurementOf` | IRI | 0...1 | O | `http://data.europa.eu/dr8/SpatialResolution` |
| [Spatial Resolution as Scale](#9.1.2-spatial-resolution-as-scale) | `geodcat:spatialResolutionAsScale` | `xsd:decimal` | 1 | M (vector) | |
| [Unit of Measure](#9.1.3-unit-of-measure) | `sdmx-attribute:unitMeasure` | `skos:Concept` | 1 | R (raster) | |
| [Value of Measurement](#9.1.4-value-of-measurement) | `dqv:value` | `xsd:decimal` | 1 | M (raster) | |

------

### 9.1 Field-by-field specification

#### 9.1.1 Measured Scalar Quantity

- **Form element**: Read-only input or select
- **Description**: The quality dimension being measured (typically Spatial Resolution).
- **Cardinality**: 0...1
- **Datatype**: IRI
- **Mapped to**: `dqv:isMeasurementOf`
- **Example**:
  ```json
  {
    "dqv:isMeasurementOf": { "@id": "http://data.europa.eu/dr8/SpatialResolution" }
  }
  ```

#### 9.1.2 Spatial Resolution as Scale

- **Form element**: Number input
- **Description**: The scale denominator of the dataset (e.g., 5000 for 1:5000).
- **Cardinality**: 1 (Mandatory for vector resources)
- **Datatype**: `xsd:decimal`
- **Mapped to**: `geodcat:spatialResolutionAsScale`
- **Example**:
  ```json
  {
    "geodcat:spatialResolutionAsScale": { "@value": "5000", "@type": "xsd:decimal" }
  }
  ```

#### 9.1.3 Unit of Measure

- **Form element**: Select list (controlled vocabulary)
- **Description**: The unit of measurement for the resolution (for raster data).
- **Cardinality**: 1 (Recommended for raster resources)
- **Datatype**: `skos:Concept`
- **Mapped to**: `sdmx-attribute:unitMeasure`
- **Example**:
  ```json
  {
    "sdmx-attribute:unitMeasure": { "@id": "http://publications.europa.eu/resource/authority/unit/M" }
  }
  ```

#### 9.1.4 Value of Measurement

- **Form element**: Number input
- **Description**: The resolution value or distance (for raster data).
- **Cardinality**: 1 (Mandatory for raster resources)
- **Datatype**: `xsd:decimal`
- **Mapped to**: `dqv:value`
- **Example**:
  ```json
  {
    "dqv:value": { "@value": "10.0", "@type": "xsd:decimal" }
  }
  ```

------

## 10. `vcard:Organization` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for defining Contact Point descriptions using `vcard:Organization`.

| Field | JSON-LD Property | Range Type | Cardinality | DCAT-BE Level | Predefined Value(s) |
|---|---|---|---|---|---|
| [Name](#10.1.1-name) | `vcard:organization-name` | `rdf:langString` | 1...n | M | |
| [Email](#10.1.2-email) | `vcard:hasEmail` | URI (`mailto:`) | 0...1 | R | |
| [Website](#10.1.3-website) | `vcard:hasURL` | IRI | 0...4 | R | |
| [Address](#10.1.4-address) | `vcard:hasAddress` | `vcard:Address` | 0...1 | O | |

### 10.1 Field-by-field specification

#### 10.1.1 Name
- **Form element**: text input (per language)
- **Description**: The name of the organization.
- **Cardinality**: 1...n
- **Mapped to**: `vcard:organization-name`

#### 10.1.2 Email
- **Form element**: email input
- **Description**: The email address of the contact point.
- **Cardinality**: 0...1
- **Mapped to**: `vcard:hasEmail`
- **Example**: `{ "vcard:hasEmail": { "@id": "mailto:info@example.be" } }`

#### 10.1.3 Website
- **Form element**: URL input
- **Description**: The website of the organization.
- **Cardinality**: 0...4
- **Mapped to**: `vcard:hasURL`

#### 10.1.4 Address
- **Form element**: nested group
- **Description**: The postal address of the organization.
- **Sub-properties** (Mandatory if address provided):
  - `vcard:street-address`: Street and number.
  - `vcard:locality`: City/Municipality.
  - `vcard:postal-code`: Postal code.
  - `vcard:country-name`: Country.

------

## 11. `foaf:Organization` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for defining Publisher or Agent descriptions using `foaf:Organization`.

| Field | JSON-LD Property | Range Type | Cardinality | DCAT-BE Level | Predefined Value(s) |
|---|---|---|---|---|---|
| [Name](#11.1.1-name) | `foaf:name` | `rdf:langString` | 1...n | M | |
| [Type](#11.1.2-type) | `dcterms:type` | `skos:Concept` | 0...1 | O | |
| [Email](#11.1.3-email) | `foaf:mbox` | URI (`mailto:`) | 0...1 | R | |
| [Website](#11.1.4-website) | `foaf:workplaceHomepage` | IRI | 1...4 | R | |
| [Address](#11.1.5-address) | `locn:address` | `locn:Address` | 0...1 | O | |

### 11.1 Field-by-field specification

#### 11.1.1 Name
- **Form element**: text input (per language)
- **Description**: The name of the agent.
- **Cardinality**: 1...n
- **Mapped to**: `foaf:name`

#### 11.1.2 Type
- **Form element**: select list (controlled vocabulary)
- **Description**: The type of the agent (e.g., Company, NGO, etc.).
- **Cardinality**: 0...1
- **Mapped to**: `dcterms:type`

#### 11.1.3 Email
- **Form element**: email input
- **Description**: The public email mailbox of the agent.
- **Cardinality**: 0...1
- **Mapped to**: `foaf:mbox`

#### 11.1.4 Website
- **Form element**: URL input
- **Description**: The official website of the agent.
- **Cardinality**: 1...4
- **Mapped to**: `foaf:workplaceHomepage`

#### 11.1.5 Address
- **Form element**: nested group
- **Description**: The postal address of the agent using the `locn` vocabulary.

------

## 12. JSON-LD Output

### 12.1 Context

```json
{
  "@context": {
      "adms": "http://www.w3.org/ns/adms#",
      "dcat": "http://www.w3.org/ns/dcat#",
      "dcterms": "http://purl.org/dc/terms/",
      "dct": "http://purl.org/dc/terms/",
      "dcmi": "http://purl.org/dc/dcmitype/",
      "dqv": "http://www.w3.org/ns/dqv#",
      "foaf": "http://xmlns.com/foaf/0.1/",
      "geodcat": "http://data.europa.eu/930/",
        "sdmx-attribute": "http://purl.org/linked-data/sdmx/2009/attribute#",
        "vcard": "http://www.w3.org/2006/vcard/ns#",
        "xsd": "http://www.w3.org/2001/XMLSchema#"
  }
}
```

### 12.2 Example

```json
{
    "@context": {
        "adms": "http://www.w3.org/ns/adms#",
        "dcat": "http://www.w3.org/ns/dcat#",
        "dcterms": "http://purl.org/dc/terms/",
        "dct": "http://purl.org/dc/terms/",
        "dcmi": "http://purl.org/dc/dcmitype/",
        "dqv": "http://www.w3.org/ns/dqv#",
        "foaf": "http://xmlns.com/foaf/0.1/",
        "geodcat": "http://data.europa.eu/930/",
        "gsp": "http://www.opengis.net/ont/geosparql#",
        "locn": "http://www.w3.org/ns/locn#",
        "sdmx-attribute": "http://purl.org/linked-data/sdmx/2009/attribute#",
        "vcard": "http://www.w3.org/2006/vcard/ns#",
        "xsd": "http://www.w3.org/2001/XMLSchema#"
    },
    "@id": "https://www.static.favv.be/bo-documents/inter_liste_smiley",
    "@type": "dcat:Dataset",
    "dcterms:title": [
        {
            "@value": "Lijst Smileys",
            "@language": "nl"
        },
        {
            "@value": "Liste Smileys",
            "@language": "fr"
        },
        {
            "@value": "List of Smileys",
            "@language": "en"
        }
    ],
    "dcterms:description": [
        {
            "@value": "De lijst van Smileys geeft een lijst van alle bedrijven die op dit ogenblik een smiley hebben. De smiley is een zelfklever die aantoont dat het bedrijf een geloofwaardig systeem van hygiëne toepast.",
            "@language": "nl"
        },
        {
            "@value": "La liste de Smileys reprend toutes les entreprises qui possèdent momenteel un smiley. Le smiley est un autocollant attestant que l'entreprise applique un système d'hygiène digne de foi.",
            "@language": "fr"
        },
        {
            "@value": "The Smileys list includes all companies that currently have a smiley face. The smiley is a sticker attesting that the company applies a reliable hygiene system.",
            "@language": "en"
        }
    ],
    "dcterms:identifier": "favv-smileys",
    "dcterms:accessRights": {
        "@id": "http://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess/noLimitations"
    },
    "dcterms:license": {
        "@type": "dcterms:LicenseDocument",
        "dcterms:type": "dcmi:Text",
        "dcterms:title": [
            {
                "@value": "Licentietitel",
                "@language": "nl"
            }
        ],
        "dcterms:description": {
            "@language": "nl",
            "@value": "Open data zijn openbare, niet persoonsgebonden gegevens die in een machinaal leesbaar formaat worden aangeboden en gratis te hergebruiken zijn, zowel voor commercieel als niet-commercieel gebruik, op voorwaarde dat de gebruiker de bron en de datum van laatste bijwerking vermeldt. De inhoud van de hergebruikte informatie mag niet misleidend zijn. In het bijzonder mag deze inhoud geen aanleiding geven tot de veronderstelling dat de gebruiker verbonden is met, de steun heeft van, goedgekeurd is door of een officieel statuut heeft verkregen van het FAVV."
        }
    },
    "dcat:theme": [
        {
            "@id": "https://vocab.belgif.be/auth/datatheme/HEAL"
        },
        {
            "@id": "https://vocab.belgif.be/auth/datatheme/ECON"
        }
    ],
    "dcat:distribution": [
        {
            "@id": "https://favv-afsca.be/nl/open-data/distribution/smileys-csv",
            "@type": "dcat:Distribution",
            "dcterms:title": {
                "@value": "Lijst Smileys (CSV)",
                "@language": "nl"
            },
            "dcterms:format": {
                "@id": "http://publications.europa.eu/resource/authority/file-type/CSV"
            },
            "dcat:accessURL": {
                "@id": "https://favv-afsca.be/nl/open-data"
            },
            "dcat:downloadURL": {
                "@id": "https://www.static.favv.be/bo-documents/inter_liste_smiley.csv"
            },
            "dcat:mediaType": {
                "@id": "https://www.iana.org/assignments/media-types/text/csv"
            }
        }
    ],
    "dcterms:publisher": {
        "@id": "https://favv-afsca.be",
        "@type": "foaf:Organization",
        "foaf:mbox": "mailto:center.contact@favv-afsca.be",
        "foaf:workplaceHomepage": {
            "@id": "https://favv-afsca.be"
        },
        "foaf:name": [
            {
                "@value": "Federaal Agentschap voor de veiligheid van de voedselketen",
                "@language": "nl"
            },
            {
                "@value": "Agence fédérale pour la sécurité de la chaîne alimentaire",
                "@language": "fr"
            },
            {
                "@value": "Federal Agency for the Safety of the Food Chain",
                "@language": "en"
            }
        ],
        "locn:address": {
            "@type": "locn:Address",
            "locn:adminUnitL1": [
                {
                    "@value": "België",
                    "@language": "nl"
                },
                {
                    "@value": "Belgique",
                    "@language": "fr"
                },
                {
                    "@value": "Belgium",
                    "@language": "en"
                }
            ],
            "locn:thoroughfare": [
                {
                    "@value": "Kruidtuinlaan 55",
                    "@language": "nl"
                }
            ],
            "locn:postName": [
                {
                    "@value": "Brussel",
                    "@language": "nl"
                }
            ],
            "locn:postCode": "1000"
        }
    ],
    "dcterms:modified": { "@value": "2026-02-23", "@type": "xsd:date" },
    "dcterms:created": { "@value": "2012-06-01", "@type": "xsd:date" },
    "dcterms:spatial": {
        "@type": "dcterms:Location",
        "skos:prefLabel": [
            {
                "@value": "België",
                "@language": "nl"
            },
            {
                "@value": "Belgique",
                "@language": "fr"
            },
            {
                "@value": "Belgium",
                "@language": "en"
            }
        ],
        "dcterms:identifier": { "@id": "http://publications.europa.eu/resource/authority/country/BEL" },
        "dcat:bbox": {
            "@type": "gsp:wktLiteral",
            "@value": "POLYGON((2.51357303225 49.5294835476, 6.15665815596 49.5294835476, 6.15665815596 51.4750237087, 2.51357303225 51.4750237087, 2.51357303225 49.5294835476))"
        }
    },
    "dcat:contactPoint": {
        "@type": "vcard:Organization",
        "vcard:fn": "FAVV Center Contact",
        "vcard:hasEmail": {
            "@id": "mailto:center.contact@favv-afsca.be"
        }
    },
    "dcterms:accrualPeriodicity": {
        "@id": "http://publications.europa.eu/resource/authority/frequency/WEEKLY"
    },
    "dcterms:language": [
        {
            "@id": "http://publications.europa.eu/resource/authority/language/NLD"
        },
        {
            "@id": "http://publications.europa.eu/resource/authority/language/FRA"
        },
        {
            "@id": "http://publications.europa.eu/resource/authority/language/ENG"
        },
        {
            "@id": "http://publications.europa.eu/resource/authority/language/DEU"
        }
    ],
    "dcat:landingPage": {
        "@id": "https://favv-afsca.be/nl/open-data"
    }
}
```

------

## 13. Validation

The output must:

- Be valid JSON-LD ([validation playground](https://json-ld.org/playground/))
- Include all **DCAT-BE Mandatory** properties
- Be validated against the [DCAT-BE SHACL shape](https://github.com/belgif/inspire-dcat/blob/main/bedcatap2.shacl) ([SHACL playground](https://shacl.org/playground/))
