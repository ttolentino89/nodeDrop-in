# nodeDrop-in

<img width="1054" alt="Screen Shot 2020-07-14 at 3 28 49 PM" src="https://user-images.githubusercontent.com/27389714/87468023-de3e5b80-c5e6-11ea-9878-d0a9f8cf7c91.png">

This demo follows the official [Drop-in web tutorial](https://docs.adyen.com/checkout/drop-in-web/tutorial-node-js) as well as the [Node.js + Express Example](https://github.com/adyen-examples/adyen-node-online-payments) from the Adyen engineering team. Drop-in is built upon [Adyen's API Library](https://github.com/Adyen/adyen-node-api-library), which you can learn more about [here](https://docs.adyen.com/api-explorer/#/PaymentSetupAndVerificationService/v52/overview).

## The goal:
Implement the Adyen Drop-in solution, and process some test payments.

## The starting process:
- Step 1: Generate origin keys

Since the Drop-in component is primarily powered by API responses, I figured I would follow the directions outlined in the guide to [Generating Multiple Origin Keys](https://docs.adyen.com/user-management/how-to-get-an-origin-key#generate-multiple-origin-keysTest) and generate a response in Postman using the provided API Key and potentially also the Merchant Account information as headers. I knew that I would only be testing payments locally, so I really only needed to generate a key for https://localhost:8080 as my origin domain. However, when testing this in Postman, I would receive this error message:

<img width="454" alt="Screen Shot 2020-07-14 at 3 35 27 PM" src="https://user-images.githubusercontent.com/27389714/87472253-a2f35b00-c5ed-11ea-8deb-567f2879c986.png">

I had originally thought that there might have been an error with the API key or merchant account name, however testing these credentials by entering the following code in the terminal returned a positive response as well as a valid origin key. In fact, I didn't even need the merchant account name yet this early in the process.

```
curl https://checkout-test.adyen.com/v1/originKeys \
-H "X-API-key: "[******** API KEY REDACTED *********]" \
-H "Content-Type: application/json" \
-d '{
   "originDomains":[
      "http://www.localhost:8080",
   ]
}'
```

Once the origin key was generated, we were pretty much good to go as that was the only missing piece credential-wise. Upon further research, I actually discovered that Adyen actually has its own [OpenAPI specification](https://github.com/Adyen/adyen-openapi) that I could have imported into Postman. You can learn more about this [here](https://www.adyen.com/blog/improving-the-developer-experience-with-openapi-specification).


## The implementation process:

- Step 2. Add credentials + Adyen scripts

## The testing process:

"Dummy" test card numbers are available from Adyen [here](https://docs.adyen.com/development-resources/test-cards/test-card-numbers) and were used to process some test payments to make sure that checkout endpoints were functional and in proper working order. Before testing Drop-in on the "website", I tested server endpoints in the terminal by sending requests to the API such as this one below:

```
curl https://checkout-test.adyen.com/v52/paymentMethods \
-H "x-API-key: "[******** API KEY REDACTED *********]" \
-H "content-type: application/json" \
-d '{
  "merchantAccount": "[****** MERCHANT ACCOUNT REDACTED ******]",
  "countryCode": "US",
  "amount": {
    "currency": "USD",
    "value": 1500
  },
  "channel": "Web",
  "shopperLocale": "us-US"
}'
```

To make sure everything was working on the front-end UI after the "back-end" API calls returned positive responses, I tested the UI with similar test payments as illustrated below:


<img src="dropin/public/images/adyendropin.gif">


## Observations:

I had actually originally wanted to try implementing this in Ruby (on Rails) since it's a language I don't have much experience in but would love to practice, but it appears there might be an issue with the example repo.  

## Special thanks to

Ms. Leslie Cruz (Implementation Team Lead) & Ms. Casey Roth (US Recruiter) for giving me the chance to work on this assignment and also for having faith in me for getting this far! I had a blast working on this exercise and it was definitely a fun learning experience, so thank you again for everything!

