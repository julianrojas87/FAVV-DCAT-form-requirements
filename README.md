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



> *More recent versions of these standards currently exist, but the DCAT-BE specifications is limited to these older versions, which must be followed for compliance.   

#### Common prefixes:

| Prefix  | **Namespace IRI**                           |
| ------- | ------------------------------------------- |
| belgif  | https://vocab.belgif.be/auth/datatheme/     |
| dcat    | http://www.w3.org/ns/dcat#                  |
| dcterms | http://purl.org/dc/terms/                   |
| foaf    | http://xmlns.com/foaf/0.1/                  |
| rdf     | http://www.w3.org/1999/02/22-rdf-syntax-ns# |
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

| Field                             | JSON-LD Property       | Range Type                                         | Cardinality | DCAT-BE Level | Predefined Value(s)                                          |
| --------------------------------- | ---------------------- | -------------------------------------------------- | ----------- | ------------- | ------------------------------------------------------------ |
| [Dataset IRI](#4.1.1-dataset-iri) | `@id`                  | IRI                                                | 1           | M             |                                                              |
| [Type](#4.1.2-type)               | `@type`                | IRI                                                | 1           | M             | `dcat:Dataset`                                               |
| [Title](#4.1.3-title)             | `dcterms:title`        | `rdf:langString`                                   | 1           | M             |                                                              |
| [Description](#4.1.4-description) | `dcterms:description`  | `rdf:langString`                                   | 1           | M             |                                                              |
| Identifier                        | `dcterms:identifier`   | `xsd:string`                                       | 1           | M             |                                                              |
| Access Rights                     | `dcterms:accessRights` | IRI                                                | 1           | M             | [see here](https://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess) |
| License                           | `dcterms:license`      | IRI                                                | 1           | M             |                                                              |
| Theme                             | `dcat:theme`           | IRI                                                | 1..n        | M             | `belgif:AGRI`                                                |
| Distribution                      | `dcat:distribution`    | [`dcat:Distribution`](#5-dcatdistribution-fiel;ds) | 0..n        | R             |                                                              |
| Publisher                         | `dcterms:publisher`    | IRI                                                | 0..n        | R             |                                                              |
| Modified Date                     | `dcterms:modified`     | `xsd:date`                                         | 0..1        | R             |                                                              |
| Created Date                      | `dcterms:created`      | `xsd:date`                                         | 0..1        | R             |                                                              |
| Spatial Coverage                  | `dcterms:spatial`      | `dcterms:Location`                                 | 0..n        | R             |                                                              |
| Temporal Coverage                 | `dcterms:temporal`     | `dcterms:PeriodOfTime`                             | 0..n        | R             |                                                              |
| Landing Page                      | `dcat:landingPage`     | IRI                                                | 0..n        | R             | https://favv-afsca.be/nl/open-data                           |
| Contact Point                     | `dcat:contactPoint`    | `vcard:Organization`                               | 0..n        | O             |                                                              |
| Issued Date                       | `dcterms:issued`       | `xsd:date`                                         | 0..1        | O             |                                                              |
| Keyword                           | `dcat:keyword`         | `rdf:langString`                                   | 0..n        | O             |                                                              |

------

### 4.1 Field-by-Field Specifications

#### 4.1.1 Dataset IRI

- **Form element**: IRI

- **Description**: Global Web identifier of the dataset.

- **cardinality**: 1

- **Mapped to**: `@id`

- **Notes**: The form must validate that a well-formed IRI is given. Ideally it should be a dereference-able IRI.

- **Example**:

  ```json
  {
      "@id": "https://favv-afsca.be/nl/open-data/inter_liste_smiley"
  }
  ```

#### 4.1.2 Type

* **Form element**: IRI

* **Description**: Type identifier, according to the DCAT standard.

* **cardinality**: 1

* **Mapped to**: `@type`

* **Predefined value**: `dcat:Dataset`

* **Example**:

  ```json
  {
      "@type": "dcat:Dataset"
  }
  ```

#### 4.1.3 Title

* **Form element**: `xsd:langString`

* **Description**: Title of the dataset considered.

* **cardinality**: 1..n

* **Mapped to**: `dcterms:title`

* **Multilingual**: `nl`, `fr`, `de`, `en`

* **Example**:

  ```json
  {
    "dcterms:title": [
      { "@value": "Titel in het Nederlands", "@language": "nl" },
      { "@value": "Titre en français", "@language": "fr" },
      { "@value": "Titel auf Deutsch", "@language": "de" },
      { "@value": "Title in English", "@language": "en" }
    ]
  }
  ```

#### 4.1.4 Description

* **Form element**: `xsd:langString`

* **Description**: Description of the dataset considered.

* **cardinality**: 1

* **Mapped to**: `dcterms:description`

* **Multilingual**: `nl`, `fr`, `de`, `en`

* **Example**:

  ```json
  {
      "dcterms:description": [
          { "value": "beschrijving in het nederlands", "@language": "nl"},
          { "value": "title in english", "@language": "fr"},
          { "value": "title in english", "@language": "de"},
          { "value": "description in english", "@language": "en"},
      ]
  }
  ```

#### 4.1.2 Identifier

- **Form element**: text
- **Mapped to**: `dcterms:identifier`
- **Datatype**: string

#### 4.1.3 Multi-language Title / Description

- **Form element**: repeatable text + language selector
- **Mapped to**: `dcterms:title`, `dcterms:description`
- **Datatype**: language tagged string

#### 4.1.4 Publisher (Agent)

- **Form element**: entity reference (organization) or URI
- **Mapped to**: `dcterms:publisher`
- **Expected type**: foaf:Organization

#### 4.1.5 License

- **Form element**: select list
- **Mapped to**: `dcterms:license`
- **Datatype**: IRI to license term (e.g., Creative Commons)

#### 4.1.6 Dates

Provide **Issued**, **Modified**, and/or **Created**. DCAT-BE wants at least one of these. 

- **Form element**: date picker

------

## 5. `dcat:Distribution` fields

Below is the field list including Belgian DCAT-AP requirement levels and cardinalities, for creating a DCAT Distribution description.

### 5.1 Field-by-field specification





## 6. JSON-LD Output

### 6.1 Context

```
{
  "@context": {
    "dcat": "http://www.w3.org/ns/dcat#",
    "dct":  "http://purl.org/dc/terms/",
    "foaf": "http://xmlns.com/foaf/0.1/",
    "adms": "http://www.w3.org/ns/adms#",
    "xsd":  "http://www.w3.org/2001/XMLSchema#"
  }
}
```

### 6.2 Example

```
{
  "@context": { ... },
  "@id": "https://data.example.be/dataset/traffic-counts",
  "@type": "dcat:Dataset",
  "dcterms:identifier": "traffic-counts-2025",
  "dcterms:title": [
    { "@value": "Traffic Counts 2025", "@language": "en" }
  ],
  "dcterms:description": [
    { "@value": "Road traffic counts collected by ...", "@language": "en" }
  ],
  "dcterms:publisher": { "@id": "https://data.example.be/org/transport" },
  "dcterms:license": { "@id": "https://creativecommons.org/licenses/by/4.0/" },
  "dcterms:issued": { "@value": "2025-01-10", "@type": "xsd:date" },
  "dcat:keyword": [
    { "@value": "traffic", "@language": "en" },
    { "@value": "mobility", "@language": "en" }
  ]
}
```

------

## 7. Validation

The output must:

- Be valid JSON-LD ([validation playground](https://json-ld.org/playground/))
- Include all **DCAT-BE Mandatory** properties
- Be validated against the [DCAT-BE SHACL shape](https://github.com/belgif/inspire-dcat/blob/main/bedcatap2.shacl) ([SHACL playground](https://shacl.org/playground/))

------

## 8. Next Steps

If you want, provide the **annex tables from the DCATAPprofil.en.md** file so I can integrate *all* recommended and optional fields with exact requirement levels and controlled vocabularies.
