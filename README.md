# Contracts Completion

The Open Contracting Data Standard can be used to provide information on all stages of a contracting process, from planning through to implementation.

This extension introduces four fields that can be used at the end of a contracting process to provide details of the final date and value of the contract, and, where there is variation, to provide a justification of this.

## Additional fields

The fields introduced by this extension are:

* **`implementation.endDate`** - The date when contract implementation ended. This should only be provided when contract implementation is complete. Where `implementation.endDate` varies from the anticipated `contracts.period.endDate` an explanation of the variance should be provided in `implementation.endDateDetails`.

* **`implementation.endDateDetails`** - Details related to the endDate. This may be a justification for the contract's completion date being different than in the original contract.

* **`implementation.finalValue`** - The total value of all payments for a completed contract. This should only be provided when the final payment has been recorded. If `implementation.transactions` are used for this contract, this field should equal the sum of the `transactions.value.amount` fields. Where `finalValue.amount` varies from `contracts.value.amount` an explanation of the variance should be provided in `finalValueDetails`.

* **`implementation.finalValueDetails`** - Details related to the final value. This may be a justification for the completed contract's value being different than in the original contract.

## Using existing OCDS fields within a Contracts Register

OCDS contains many existing fields that can be used as part of a Contracts Register. These are documented [in the schema reference](http://standard.open-contracting.org/latest/en/schema/reference/). This extension does not modify any of these fields. However, the following list is provided for convenience of those considering the design of a contracts register:

* **Supplier details**  should be recorded within the `awards` section, linked via `contracts.awardID` (even if you are only releasing information at the contract stage, you may provide information in the tender and award sections)
* **Contract documents** can be linked to under `contracts.documents`
* **Performance reports** can be provided under `contracts.implementation.documents`
* **Details of payments** can be provided under `contracts.implementation.transactions`
* **Progress details** can be provided using `contracts.implementation.milestones`.
* **Amendments** can be described using the `contracts.amendments` array, and with past values provided using the OCDS [releases model as described here](http://standard.open-contracting.org/latest/en/implementation/amendments/)

### Using milestones to show contract completion

Milestones may have a `status` of 'scheduled', 'met', 'notMet' or 'partiallyMet'. By providing at least one milestone for a contract, and then ensuring `milestones.status` is updated when `implementation.endDate` you can indicate whether a contract ended with successful delivery of all milestones and deliverables.

## JSON and CSV serializations

In some cases, it may be possible to design a simple contracts register using the [flat CSV serialization of OCDS](http://standard.open-contracting.org/latest/en/implementation/serialization/#csv).

## Example

The following extract illustrates these properties in use within the `contracts.implementation` block. Note the difference between the contract `period` and `value` (as agreed in the contract, or amended contract), and the implementation `finalValue` and `endDate`, along with the explanation provided of this variance.

```json
{
  "contracts": [
    {
      "id": "1",
      "awardID": "1",
      "title": "Contract to build new cycle lanes in the centre of town.",
      "period": {
        "startDate": "2010-07-01T00:00:00Z",
        "endDate": "2012-01-01T23:59:00Z",
        "maxExtentDate": "2012-01-31T23:59:00Z"
      },
      "value": {
        "amount": 11500000,
        "currency": "GBP"
      },
      "implementation": {
        "endDate": "2012-02-01T00:00:00Z",
        "endDateDetails": "Project was completed one day beyond the extended deadline.",
        "finalValue": {
          "amount": 11800000,
          "currency": "GBP"
        },
        "finalValueDetails": "The final payment to the supplier included a compensation payment triggered by the local authority failure to provide work permits on schedule."
      }
    }
  ]
}
```

The example folder contains a full worked example with:

* A release that provides details of a contract;
* A release that includes an amendment to the contract to increase the total value, as well as initial payment transactions;
* A release that contains a confirmed end date, final value, and the explanation of variation in these.

This is also supplied in record form, and with a simplified flat serialization. The record can be viewed with [OCDS Show](https://open-contracting.github.io/ocds-show/) to demonstrate how the OCDS releases and records model captures change over time.

In the spreadsheet serialization it is possible to see three releases describing the three key moments from the same contracting process.

## Issues

Report issues for this extension in the [ocds-extensions repository](https://github.com/open-contracting/ocds-extensions/issues), putting the extension's name in the issue's title.

## Changelog

This extension was originally discussed in <https://github.com/open-contracting/standard/issues/703>.
