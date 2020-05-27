# Gig Worker Safety Net

## Overview
You can use this drop-in module to offer your users Kover's `Gig Worker Safety Net` membership, without leaving your website. Offering relevant benefits and perks to your users shows that you care about them while also helps you earn commission for each membership sold. Information about what the membership includes can be found on https://www.kover.ai.

<center>
<img src="https://i.ibb.co/F8sBdwb/Screen-Shot-2020-05-26-at-1-13-51-PM.png" alt="Screen-Shot-2020-05-26-at-1-13-51-PM" border="0"/>
</center>

## API
`https://www.kover.ai/api/product/safetynet/v1/api`

## Authentication
**public_key** `string`

This is a 42 digit alphanumeric string that's paired with a private_key you'll need to retrieve customer data from the server side.

**argyle_token** `string` (optional)

If the users are expected to link any of the Gig App accounts, Kover evokes the `Arglye` module. By default, we use our own Argyle credentials. If you wish to use your own Argyle credential, you can pass your Argyle `pluginKey` here.

```
<!doctype html>
<html>
  <head>
      <meta charset="utf-8"/>
  </head>
  <body>
    <script src="https://kover.ai/api/product/safetynet/v1/js"></script>
    <script type="text/javascript">
      const kover = Kover.create({
        apiHost: 'https://www.kover.ai/api/product/safetynet/v1',
        koverKey: 'public_key',
        argyleKey: 'pluginKey',
        data: {
          "has_argyle": true,             // whether or not they linked Argyle account through Gridwise
          "email": "example@example.com", // this is their email with you (unique identifier)
          "monthly_income": 3289,
          "argyle_info": {
            "uber": {
              "first_name": "",
              "last_name": "",
              "phone_number": "",
              "email": "",                // this is their email address with gig Apps, based on Argyle response
              "address": "",
              "birthday": ""
            }
          }
        }
      });
      kover.open();
    </script>
  </body>
</html>
```

## Parameters

**email** `string` (optional)

User's email address. If this is not provided, the module will ask the user to enter an email address.

**income** `float` (optional)

If you already know the user's income level, you can pass it to the module here. If this is given, the user will be prompted to confirm the number. If this parameter is not passed, we'll ask the user to manually enter this info.

**benefits** `[string]`

Benefits included in the membership, expressed as a list of strings. The values will be the following: `accident`, `hospitalization`, `sick_leave` and `deactivation`

**review_benefit** `boolean`

If set to true, display the benefits as a slide show for the customers to review.

**perks** `[string]`
Perks included in the membership, expressed as a list of strings. The value will be the following: `hurdlr`, `fonemed`, `legalrideshare_deactivation`, `legalrideshare_consult` and `kover_earning_dashboard`.

**review_perks** `boolean`

If set to true, display the perks as a slide show for the customers to review. If set to true, perks will always be displayed after benefits.

**gig_accounts** `[string]`

List of gig accounts that the customers will be allowed to link. The value will be the following: `uber`, `lyft`, `instacart`, `postmates`, `doordash`, `grubhub`, `amazon_flex`, `shipt`, `cavier`, `wonolo`.

**new_payment_method** `boolean`

If set to true, the user will be prompted to enter their payment method when enrolling. If you have set up direct payment with Kover,this field can be set to `False` and when the user enrolls, we'll deduct the premium from your payment method on file and you can deduct the premium from the customer's account directly.

**redirect_to** `string`

This is the URL that the user will be directed to after successful enrollment.
