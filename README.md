# Gig Worker Safety Net

## 1. Checkout Flow

### 1.1 Overview
You can use this drop-in module to offer your users Kover's `Gig Worker Safety Net` membership, without leaving your website. Offering relevant benefits and perks to your users shows that you care about them while also helps you earn commission for each membership sold. Information about what the membership includes can be found on https://www.kover.ai.

<center>
<img src="https://i.ibb.co/F8sBdwb/Screen-Shot-2020-05-26-at-1-13-51-PM.png" alt="Screen-Shot-2020-05-26-at-1-13-51-PM" border="0"/>
</center>

### 1.2 API
`https://www.kover.ai/api/product/safetynet/v1/api`

### 1.3 Authentication
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

### 1.4 Parameters

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

## 2. Track API

### 2.1 Overview
You can view the performance of your module and retrieve information about the members you succesfully refer using the Track API. 

### 2.2 API
`https://www.kover.ai/api/product/safetynet/v1/track`

### 2.3 Authentication
**private_key** `string`

This is a 64-digit alphanumeric string that's paired with the publick key you used to activate the checkout flow module.

### 2.4 Endpoints

**/logs** `GET`

Sample Output:
```
{
  "status": "ok",
  "logs": {
    "2020-05-11T20:18:00Z": 12,   // number of seconds the module was activated
    "2020-05-12T3:19:34Z": 239, 
    ""
  }
}
```

**/pipeline** `GET`

Sample Output:
```
{
  "status": "ok",
  "leads": {
    "example@exmaple.com": "2020-05-12T3:19:34Z"
  }
}
```

**/members** `GET`

Sample Output
```
{
  "status": "ok",
  "members": {
    "0x89205A3A3b2A69De6Dbf7f01ED13B2108B2c43e7": {
      "profile": {
        "first_name": "",
        "last_name": "",
        "email": "",
        "enroll_ts": "",
        "status": "active"
      },
      "benefits": {
        "accident": "active",
        "hospitalization": "active"
      },
      "perks": {
        "hurdlr": "inactive",
        "fonemed": "active"
      },
      "claims": {
      },
      "perk_utilization": {
        "fonemed": ["2020-03-14T3:19:34Z", "2020-05-12T3:19:34Z"]
      }
    }
  }
}
```

