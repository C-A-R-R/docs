---
tap: "zuora"
version: 1.0

name: "refund"
doc-link: https://live-www.zuora.com/developer/api-reference/#tag/Refunds
description: |
  The `refund` table contains info about refunds, or transactions where money is returned to a customer.

replication-method: "Incremental"

attributes:
  - name: "id"
    type: "string"
    primary-key: true
    description: "The refund ID."

  - name: "updatedDate"
    type: "date-time"
    replication-key: true
    description: "The date when the refund was last updated."

  - name: "accountId"
    type: "string"
    description: "The ID of the account associated with the refund."
    ## foreign-keys:
    ##   - table-name: "account"
    ##     attribute: "id"

    ##   - table-name: "contact"
    ##     attribute: "accountId"

    ##   - table-name: "invoice"
    ##     attribute: "accountId"

    ##   - table-name: "payment"
    ##     attribute: "accountId"

    ##   - table-name: "subscription"
    ##     attribute: "accountId"

  - name: "amount"
    type: "number"
    description: "The total amount of the refund."

  - name: "cancelledOn"
    type: "date-time"
    description: "The date when the refund was cancelled."

  - name: "comment"
    type: "string"
    description: "The comments about the refund."

  - name: "createdById"
    type: "string"
    description: "The ID of the Zuora user who created the refund."

  - name: "createdDate"
    type: "date-time"
    description: "The date the refund was created."

  - name: "creditMemoId"
    type: "string"
    description: "The ID of the credit memo that is refunded."

  - name: "deleted"
    type: "boolean"
    description: |
      **Only supported for the AQuA API.** If `true`, this record was deleted in Zuora.

  - name: "gatewayId"
    type: "string"
    description: "The ID of the gateway instance that processes the refund."

  - name: "gatewayResponse"
    type: "string"
    description: "The message returned from the payment gateway for the refund."

  - name: "gatewayResponseCode"
    type: "string"
    description: "The code returned from the payment gateway for the refund."

  - name: "gatewayState"
    type: "string"
    description: |
      The status of the refund in the gateway. Possible values are:

      - `MarkedForSubmission`
      - `Submitted`
      - `Settled`
      - `NotSubmitted`
      - `FailedToSettle`

  - name: "markedForSubmissionOn"
    type: "date-time"
    description: "The date when a refund was marked and waiting for batch submission to the payment process."

  - name: "methodType"
    type: "string"
    description: |
      The method used to issue an external refund to a customer. Possible values are:

      - `ACH`
      - `Cash`
      - `Check`
      - `CreditCard`
      - `PayPal`
      - `WireTransfer`
      - `DebitCard`
      - `CreditCardReferenceTransaction`
      - `BankTransfer`
      - `Other`

  - name: "number"
    type: "string"
    description: "The unique identification number of the refund."

  - name: "paymentId"
    type: "string"
    description: "The ID of the payment that is refunded."
    ## foreign-keys:
    ##   - table-name: "payment"
    ##     attribute: "id"

  - name: "paymentMethodId"
    type: "string"
    description: "The unique ID of the payment method that the customer used to make the refund."
    ## foreign-keys:
    ##   - table-name: "paymentMethod"
    ##     attribute: "id"

  - name: "paymentMethodSnapshotId"
    type: "string"
    description: "The unqiue ID of the payment method snapshot, which is a copy of the particular payment method used in a transaction."
    ## foreign-keys:
    ##   - table-name: "paymentMethodSnapshot"
    ##     attribute: "id"

  - name: "reasonCode"
    type: "string"
    description: "The code identifying the reason for the transaction."

  - name: "referenceId"
    type: "string"
    description: "The transaction ID returned by the payment gateway for an electronic refund."

  - name: "refundDate"
    type: "date-time"
    description: "The date when the refund takes effect."

  - name: "refundTransactionTime"
    type: "date-time"
    description: "The date when the refund was issued."

  - name: "secondRefundReferenceId"
    type: "string"
    description: "The transaction ID returned by the payment gateway if there is an additional transaction for the refund."

  - name: "settledOn"
    type: "date-time"
    description: "The date when the refund was settled in the payment processor. This field is only applicable to Spectrum gateways."

  - name: "softDescriptor"
    type: "string"
    description: "A payment gateway-specific field that maps Zuora to other gateways."

  - name: "softDescriptorPhone"
    type: "string"
    description: "A payment gateway-specific field that maps Zuora to other gateways."

  - name: "status"
    type: "string"
    description: |
      The status of the refund. Possible values are:

      - `Processed`
      - `Canceled`
      - `Error`
      - `Processing`

  - name: "submittedOn"
    type: "date-time"
    description: "The date when the refund was submitted."

  - name: "success"
    type: "boolean"
    description: "If `true`, the request was processed successfully."

  - name: "type"
    type: "string"
    description: "The type of refund, either `External` or `Electronic`."

  - name: "updatedById"
    type: "string"
    description: "The ID of the Zuora user who last updated the refund."
---