## 3.20.0 - 2022.05.12
### Added

#### Terms and conditions acceptance parameter

The acceptance of the MANGOPAY terms and conditions by the end user can now be registered via the SDK.

This information can be managed by using the new `TermsAndConditionsAccepted` parameter added to the `User` object.

The following API endpoints have been updated to take into account the new TermsAndConditionsAccepted parameter:

[Create a Natural User](https://docs.mangopay.com/endpoints/v2.01/users#e255_create-a-natural-user)
[Update a Natural User](https://docs.mangopay.com/endpoints/v2.01/users#e260_update-a-natural-user)
[Create a Legal User](https://docs.mangopay.com/endpoints/v2.01/users#e259_create-a-legal-user)
[Update a Legal User](https://docs.mangopay.com/endpoints/v2.01/users#e261_update-a-legal-user)
[View a User](https://docs.mangopay.com/endpoints/v2.01/users#e256_view-a-user)

Please note that:

- Existing users have to be updated to include the terms and conditions acceptance information.
- Once accepted, the terms and conditions cannot be revoked.


## 3.19.0 - 2022.03.31
### Added

#### Instant payment eligibility check

With the function
`PayOutEligibility(params).check_eligibility()`
the destination bank reachability can now be verified prior to making an instant payout. This results in a better user experience, as this preliminary check will allow the platform to propose the instant payout option only to end users whose bank is eligible. 


## 3.18.1 - 2022.02.18
### Fixed

- Refund for MIT/CIT has been fixed
- secure_mode parameter is not set anymore for CIT 


## 3.18.0 - 2021.11.19
## Added

### Instant payouts hooks

We are now providing new hooks for our new feature [Instant payouts](https://docs.mangopay.com/guide/instant-payment-payout) :

- INSTANT_PAYOUT_SUCCEEDED
- INSTANT_PAYOUT_FALLBACKED

It will allow you to trigger an action depends on the Instant Payout treatment.

### GET a RecurringPayIn ID

You can now request the RecurringPayIn ID to check if the status is valid using

## 3.17.0 - 2021.10.20
## Added

You can now change the status to "ENDED" for a recurring payment.

## Fixed

- "Status" is now available in the response when you request a recurring payment registration.

## 3.16.0 - 2021.10.11
## Added

**We provide more information regarding refused KYC documents.** Therefore it will be easier for you to adapt your app behavior and help your end user.

You are now able to see the exact explanation thanks to a new parameter called “Flags”. 

It has been added to 

`$this->_api->KycDocuments->Get($kycDocument->Id);`

It will display one or several error codes that provide the reason(s) why your document validation has failed. These error codes description are available [here](https://docs.mangopay.com/guide/kyc-document).

## 3.15.0 - 2021.09.30
## Added

As requested by numerous clients, we are now providing [Payconiq](https://www.payconiq.be/fr) as a new mean-of-payment. To request access, please contact MANGOPAY.

## Fixed 

+ BillingField & ShippingField are now optionals for RecurringPayIn
+ We have fixed DateTimeField (previously was generating an error due to format) 

## 3.14.1 - 2021.08.05
## Fixed 

- Change `FallbackReason` parameter's type to object in BankWirePayOut  

## 3.14.0 - 2021.08.04
## Added

- You can now update and view a Recurring PayIn Registration object. To know more about this feature, please consult the documentation [here](https://docs.mangopay.com/guide/recurring-payments-introduction). 
- To improve recurring payments, we have added new parameters for CIT : DebitedFunds & Fees. To know more about this feature, please consult the documentation [here](https://docs.mangopay.com/endpoints/v2.01/payins#e1053_create-a-recurring-payin-cit)

## 3.12.0 - 2021.06.10
## Added 

We have added a new feature **[recurring payments](https://docs.mangopay.com/guide/recurring-payments)** dedicated to clients needing to charge a card repeatedly, such as subscriptions or payments installments. 

You can start testing in sandbox, to help you define your workflow. This release provides the first elements of the full feature.

- [Create a Recurring PayIn Registration object](https://docs.mangopay.com/endpoints/v2.01/payins#e1051_create-a-recurring-payin-registration), containing all the information to define the recurring payment
- [Initiate your recurring payment flow](https://docs.mangopay.com/endpoints/v2.01/payins#e1053_create-a-recurring-payin-cit) with an authenticated transaction (CIT) using the Card Recurring PayIn endpoint
- [Continue your recurring payment flow](https://docs.mangopay.com/endpoints/v2.01/payins#e1054_create-a-recurring-payin-mit) with an non-authenticated transaction (MIT) using the Card Recurring PayIn endpoint

This feature is not yet available in production and you need to contact the Support team to request access.

## 3.11.0 - 2021.05.27
## Added 

Mangopay introduces the instant payment mode. It allows payouts (transfer from wallet to user bank account) to be processed within 25 seconds, rather than the 48 hours for a standard payout.

You can now use this new type of payout with the Python SDK.

Example :

```python
get_bank_wire = BankWirePayOut.get_bankwire(payout_id)
# where payout_id is the id of an existing payout
```

Please note that this feature must be authorized and activated by MANGOPAY. More information [here](https://docs.mangopay.com/guide/instant-payment-payout).

## Fixed

Duplicate BIC in resources 

## 3.10.1
## Fixed

## Fixed 

### IBAN for testing purposes

⚠️ **IBAN provided for testing purpose should never be used outside of a testing environement!**

- Fix `BankAccount` IBAN reference for tests

More information about how to test payments, click [here](https://docs.mangopay.com/guide/testing-payments).

### Others

- httplib2 has been updated to the last version
- RemainingFunds was flag wrongly as mandatory for PreAuthorization. It has been fixed.
- ProcessedDate had the type IntegerField instead of DateTimeField. It has been fixed.

## Added 

### New events for PreAuthorization

Some of you use a lot the [PreAuthorization](https://docs.mangopay.com/endpoints/v2.01/preauthorizations#e183_the-preauthorization-object) feature of our API. To make your life easier, we have added three new events :

- PREAUTHORIZATION_CREATED
- PREAUTHORIZATION_SUCCEEDED
- PREAUTHORIZATION_FAILED

The goal is to help you monitor a PreAuthorization with a [webhook](https://docs.mangopay.com/endpoints/v2.01/hooks#e246_the-hook-object).

*Example: If a PreAuthorization is desynchronized, when the status is updated, you will be able to know it.*

### Logging

We have merged @rbarrois pull request. The logging module expects user to provide the string as a first argument, and all interpolation parameters in separate positional or keyword arguments: `logger.debug("trying x=%s", x)`.

This brings two benefits:

- The interpolation is only performed if the logging message is actually
used (debug messages won't even be interpolated if logging is set to
WARNING)

- Monitoring libraries like Sentry can group messages based on their
non-interpolated message, which helps detecting similar issues.


## 3.10.0
## Added

### On demand feature for 3DSv2

> **This on-demand feature is for testing purposes only and will not be available in production**

#### Request

We've added a new parameter `Requested3DSVersion` (not mandatory) that allows you to choose between versions of 3DS protocols (managed by the parameter `SecureMode`). Two values are available: 
* `V1`
* `V2_1`

If nothing is sent, the flow will be 3DS V1. 

The `Requested3DSVersion` may be included on all calls to the following endpoints:
* `/preauthorizations/card/direct`
* `/payins/card/direct`

#### Response

In the API response, the `Requested3DSVersion` will show the value you requested:
* `V1`
* `V2_1`
* `null` – indicates that nothing was requested

The parameter `Applied3DSVersion` shows you the version of the 3DS protocol used. Two values are possible:
* `V1`
* `V2_1`

## 3.9.0
- 3DS2 integration with Shipping and Billing objects, including FirstName and LastName fields
The objects Billing and Shipping may be included on all calls to the following endpoints:
  - /preauthorizations/card/direct
  - /payins/card/direct
  - /payins/card/web
- Enable Instant Payment for payouts by adding a new parameter PayoutModeRequested on the following endpoint /payouts/bankwire
  - The new parameter PayoutModeRequested can take two differents values : "INSTANT_PAYMENT" or "STANDARD" (STANDARD = the way we procede normaly a payout request)
  - This new parameter is not mandatory and if empty or not present, the payout will be "STANDARD" by default
  - Instant Payment is in beta all over Europe - SEPA region
- Changed date to dateTime and fixed tests
- Changed User-Agent
- Fixed typo on IpAddress, FirstName & LastName
## 3.8.4
- Added 'Regulatory' endpoint to allow checks of User Block Status
- Added support for Regulatory -> Blocked Status Hooks
## 3.8.3
- added new methods for client fees wallet bank accounts and payouts
- added new method for PreAuthorization transactions
- fixed handler sandbox false condition
- added headers to APIError
- Improve logging calls. 

## 3.8.2
- Added missing 'creation_date' field to BankWirePayOut
- Handler has been modified in save function
- New RemainingFunds Parameters (Complete feature not fully activated, please listen for product announcements)
- Added CardValidation endpoint  (Complete feature not fully activated, please listen for product announcements)
- New MultiCapture Parameter in Preauthorization object (Complete feature not fully activated, please listen for product announcements)
- Added OUT_OF_DATE status for KYC docs
- Library upgrade httplib2
- "User-agent" format in the headers changed, aligned to other assets 

## [3.8.0]
### Added
- ApplePay Payins support has been added. Feel free to ask our Customer Success / Sales team about conditions and availability.
- GooglePay Payins support has been added. Feel free to ask our Customer Success / Sales team about conditions and availability too
- `GetEmoney` method now supports year and month parameters. More info on our [docs](https://docs.mangopay.com/endpoints/v2.01/user-emoney#e895_view-a-users-emoney)
- New `Dispute` reason type `COUNTERFEIT_PRODUCT` has been added.
- `Culture` parameter has been added for `Payin Direct`
- `Mandate` status `EXPIRED` and related `EventType` `MANDATE_EXPIRED` are now supported.
### Fixed
- `EMoney` amounts for a user are now available by month
- `Dispute` reason type `COUNTERFEIT_PRODUCT` is now available.
- UBOs can now be disactivated thanks to `isActive` property support.
- `Birthday` type is now a `Date` rather than an `Integer`

## [3.7.1] - 2019-09-24
### Fixed
- Fix issue on `UBODeclaration` creation

## [3.7.0] - 2019-07-09
### Added
- new [`UBODelaration`](https://docs.mangopay.com/endpoints/v2.01/ubo-declarations#e1024_the-ubo-declaration-object) submission system
- `CompanyNumber` support for [Legal `Users`](https://docs.mangopay.com/endpoints/v2.01/users#e259_create-a-legal-user)

 