# KYC Check

Use this api to check if your customer's identity detail exists in any of the KRAs (KYC Registration Agencies) database. If it exists, you'll get certain demographic information of the investor and the status of the KYC in the response object.

#### Check kyc status
Call the KYC Check api with your customer's Income Tax Permanent Account Number (PAN)
```json
{
	"pan": "arrpp1115n"
}
```

If the `status` in the response object is `true`, it means the investor details exists in one of the KRA's databases and his KYC is under process or completed successfully. You can go ahead and onboard the investor and place purchase and redemption orders.

If the `status` is `false`, it means the investor either does not exist in any of the KRA's databases or his KYC is unsuccessful. We recommend that you do not accept any orders from the investor at this stage, as they will get rejected during the processing at the RTA.

#### Testing
In the staging environment, pan numbers that match `XXXPX3751X` are considered kyc compliant and the response object will contain `status` as true, with no constraints.  
```json
# Displaying only a part of the object for brevity
{
	"pan": "ARRPP3751N",
	"entity_details": {
		"name": "Tony Soprano"
	},
	"status": true,
	"constraints": []
}
```

Pans that match `XXXPX3752X` are considered kyc compliant with restrictions on the investment limit. The response object will contain `status` as true and constraints will be populated.  
```json
# Displaying only a part of the object for brevity
{
	"pan": "ARRPP3752N",
	"entity_details": {
		"name": "Junior Soprano"
	},
	"status": true,
	"constraints": [
        {
            "type": "investment_limit",
            "amount": {
                "value": 50000,
                "currency": "inr"
            }
        }
    ]
}
```

Any other pan number will be treated as kyc non compliant and so the response object will contain `status` as false.

```json
# Displaying only a part of the object for brevity
{
	"pan": "ARRPP1234N",
	"entity_details": {
		"name": null
	},
	"status": false,
	"constraints": []
}
```