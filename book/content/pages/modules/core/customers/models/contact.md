# Model Contact

namespace CeremonyCrmApp\Modules\Core\Customers\Models\Contact

List of contact information for a person

## Constants

This model does not define constants.

## Properties

| Property                                                                                 | Value                   |
| :--------------------------------------------------------------------------------------- | :---------------------- |
| [eloquentClass](https://docs.wai.blue/adios-framework/models/properties#eloquentClass)   | Eloquent\Contact::class |
| [table](https://docs.wai.blue/adios-framework/models/properties#table)                   | contacts                |
| [lookupSqlValue](https://docs.wai.blue/adios-framework/models/properties#lookupSqlValue) | [TABLE].value           |

## Data Scructure

| Column    | Title        | ADIOS Type                                                                 | Length | Required | Additional parameters                                                                                                 |
| --------- | ------------ | -------------------------------------------------------------------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------- |
| id        | ID           | [int](https://docs.wai.blue/adios-framework/models/attributes#int)         |        | TRUE     |                                                                                                                       |
| id_person | Person       | [lookup](https://docs.wai.blue/adios-framework/models/attributes#lookup)   |        | TRUE     |                                                                                                                       |
| type      | Contact type | [varchar](https://docs.wai.blue/adios-framework/models/attributes#varchar) |        | TRUE     | enum_values: [email => Email, phone_number => Phone Number, website => Website, linkedid => LinkedIn, fax => Fax,...] |
| value     | Value        | [varchar](https://docs.wai.blue/adios-framework/models/attributes#varchar) |        | TRUE     |                                                                                                                       |

## Foreign Keys

| Column     | Model                                                                                       | Relation | OnUpdate | OnDelete |
| ---------- | ------------------------------------------------------------------------------------------- | -------- | -------- | -------- |
| id_country | [Modules\Core\Customers\Models\Person](person.md../modules/core/customers/models/Person.md) | 1:1      | Cascade  | Restrict |

## Indexes

Only [default indexes](https://docs.wai.blue/adios-framework/default-indexes) are used.

## Relations

| Relation | Type       | Other parameters                 |
| -------- | ---------- | -------------------------------- |
| PERSON   | BELONGS_TO | Person::class, 'id_person', 'id' |
