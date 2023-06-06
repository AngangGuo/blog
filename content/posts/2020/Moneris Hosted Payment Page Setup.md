---
title: "Moneris Hosted Payment Page Setup"
date: 2020-05-01T10:41:32-07:00
categories:
 - Church
 - Payment
tags:
 - Moneris
 - Bootstrap
draft: false
---

In this COVID-19 pandemic season, to comply with new social distancing measures, 
churches around the world have sent their congregants home. 

The COVID-19 will stir those churches to seize this unprecedented opportunity to embrace change.
It is crucial for churches to lift up online giving not just as a convenience but as a theologically sound way to give.

In this article I will show you how to setup the hosted payment page from 
Moneris Gateway Merchant Resource Center.

From [Moneris](https://www.moneris.com/) website:
```
Moneris was created as a joint investment between RBC and BMO Bank of Montreal (including Harris Bank) in December 2000. 
By maintaining the tradition of security and strength of our parent banks, 
today Moneris is Canada's #1 processor, and one of North America's largest.
```
## Configure Hosted Payment Page

### Hosted Payment Page - Outline.

1. Login to Moneris Gateway Merchant Resource Center: [For development](https://esqa.moneris.com/mpg) 
1. Click on ‘Admin on the Menu
1. Click on ‘Hosted PayPage Config’ in the sub-menu
1. Generate a New configuration
1. Configure your Hosted Payment Page configuration (See below section for details)
1. Do the required development as outlined [here](https://developer.moneris.com/Documentation/NA/E-Commerce%20Solutions/Hosted%20Solutions/~/link.aspx?_id=0E57826471DC480E8F377C79BB88D6CB&_z=z)
1. Test your solution in the test environment
1. Activate your production store
1. Create and configure your production Hosted Pay Page store in the production [Merchant Resource Centre](https://www3.moneris.com/mpg)
1. Make the necessary changes to move your solution from the test environment into production

Note:
* For development: Login to the test environment using the credential listed on the web page.
* For production: Login to the production website using your own credential.
* `charge_total` must have 2 digital numbers

### Hosted Paypage Configuration
Click `Generate a New Configuration`
```
ps_store_id: 8LQ3Store1
hpp_key: hpD4DMN94PJF
```
The following message means you need to save this configuration within 15 minutes.
You can configure it after you save it.
```
This configuration is currently flagged as temporary and will be deleted in 15 minutes.
To confirm this configuration please click 'Save Changes' below.
```

#### Basic Configuration 
* Description: `AGCF - Credit Card Config - V1`
* Transaction Type: `Purchase`
* Payment Methods: `Credit Cards`
* Response Method: `Moneris Gateway will generate a receipt.`; Keep the `Approved URL` and `Declined URL` blank. 
They're for the other three response methods.
* Click `Save Changes`

#### Configure Paypage Appearance
* Hosted Paypage Data Fields: 
    * Check `Display customer details. (cust_id, email, note . . .)` <br>
    * Check `Display merchant name.`
* Buttons:
    * Cancel Button Text: `Cancel Transaction`
    * Cancel Button URL: `http://example.com/cancel.html`
    * Continue Button Text: `Continue` or `Back to AGCF`
    * Continue Button URL: `http:// example.com`
* Logos: Select the card logos to display on the payment page. Only the following cards are supported:
    * VISA
    * MC
    * Discovery
    * America Express
    * JCB
    * UnionPay 银联
* Click `Save Appearance Settings`

Please note that credit card logos are for display only and will not affect what card types you are able to accept.

### Configure Response Fields
* Response/Receipt Field Configuration: 
    * Select `Return other customer fields. (cust_id, client_email, note . . .)`
    * Select `	Automatically prompt cardholder for new card number on decline.`
    * Number of retry attempts allowed: `3`
* Asynchronous Transaction Response: Don't need server response for now.
* Save Response Settings

### Configure Security
* Referring URL: Specify the source URLs for your Hosted Paypage request. Addresses will be validated up to the file name. 
Leaving this selection blank will allow any address to post to your store.
    * Add URL: `https://example.com/donation.html`
    * Add URL: `https://www.example.com/donation.html`
    * Add URL: `https://192.168.1.1/donation.html`
* Transaction Verification: Don't need this for now
* Save Verification Settings

### Configure Email Receipts
* Receipt Conditions
	* Send email to cardholder if transaction approves.
	* Send email to cardholder if transaction declines.
	* Send email to merchant if transaction approves.
	* Merchant email address: `admin@example.com`
* Receipt Appearance: `Include customer details. (cust_id, email, note . . .)`
* If you wish for a message to appear at the top of the receipt please type the text in the provided space below.
* Save Email Settings

Email Text:
```
Thank you for your donation.
May God Bless You.
```

## Develop Donation Web Page

### Post Form Fields
Create a web page in your website and add the following code into it:
```
<FORM METHOD="POST" ACTION= "https://esqa.moneris.com/HPPDP/index.php" >
     <INPUT TYPE="HIDDEN" NAME="ps_store_id" VALUE="AF4Fs1024">
     <INPUT TYPE="HIDDEN" NAME="hpp_key" VALUE="Hsjh4GSr4g">
     <INPUT TYPE="HIDDEN" NAME="charge_total" VALUE="1.00">
     <!--MORE OPTIONAL VARIABLES CAN BE DEFINED HERE -->

    <INPUT TYPE="SUBMIT" NAME="SUBMIT" VALUE="Click to proceed to Secure Page">
</FORM>
```

Note: 
* Post Address:
    * For testing: `https://esqa.moneris.com/HPPDP/index.php`
    * For production: `https://www3.moneris.com/HPPDP/index.php`
* `ps_store-id` & `hpp_key`: See the above section on how to setup them
    * For testing: get them from `https://esqa.moneris.com/mpg`
    * For production: get them from `https://www3.moneris.com/mpg`
* Our website use iframe and the link only works by using ip address like `http://192.168.1.1/Donation/credit.html`

## Useful Links
[Using the Moneris Hosted Payment Page with data preload to reduce card testing fraud](https://www.mugo.ca/Blog/Using-the-Moneris-Hosted-Payment-Page-with-data-preload-to-reduce-card-testing-fraud)