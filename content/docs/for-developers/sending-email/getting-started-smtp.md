---
seo:
  title: How to Send an SMTP Email
  description: Use Telnet to send your first SMTP email. SendGrid’s SMTP API allows developers to specify custom handling instructions for email using an X-SMTPAPI header inserted into the message.
  keywords: telnet, ports, connection, SMTP, send email, getting started
title: How to Send an SMTP Email
group: smtp
weight: 960
layout: page
navigation:
  show: true
---

<iframe src="https://player.vimeo.com/video/190122014" width="700" height="400" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

You can also send email with [the UI]({{root_url}}/ui/sending-email/how-to-send-email-with-marketing-campaigns/) and with [the API]({{root_url}}/api-reference/).


## What is SMTP?

[SMTP]({{root_url}}/glossary/smtp/), or _simple mail transfer protocol_, is a quick and easy way to send email from one server to another. SendGrid provides an SMTP service that allows you to deliver your email via our server instead of your client or server.

SendGrid’s SMTP API allows developers to specify custom handling instructions for email using an X-SMTPAPI header inserted into the message. The header is a JSON encoded list of instructions and options for that email.

The X-SMTPAPI headers that you add are stripped from the final email because they are instruction headers for how SendGrid will handle your email.

For a deeper dive into what SMTP is, the benefits of sending an email with SMTP, and how SendGrid can help, see the [SMTP Service Crash Course](https://sendgrid.com/blog/smtp-service-crash-course/) on our blog.

## Sending a test SMTP email with Telnet


### Before you begin

- Create a SendGrid API key on the [API Keys page](https://app.sendgrid.com/settings/api_keys).
- Open your command line, bash, shell, or Terminal functionality (depending on what OS you are using). You'll use this window to input the commands to initiate a telnet connection.
- Convert your API key to Base64. It is not secure to put your API key into an external webpage for a conversion, so we recommend using a bash conversion. If you are on Mac or Linux, you can use the pre-installed OpenSSL package. Use this cmd to convert your API key using OpenSSL: `echo -n '<<YOUR_API_KEY>>' | openssl base64`. Save your converted key for a later step.

<call-out type="warning">

Telnet does not register backspaces correctly - so you have to type your commands correctly (or copy and paste it from here).

</call-out>

### To send SMTP email using Telnet:

<call-out>

If you receive this error: `'telnet' is not recognized as an internal or external command, operable program or batch file`, you need to install Telnet on your machine. Telnet comes natively on most operating systems.

</call-out>

1. Start your session by typing in the terminal: `TELNET smtp.sendgrid.net 25`.
    <br>SendGrid accepts unencrypted and TLS connections on ports **25**, **587**, & **2525**. You can also connect via SSL on port **465**.
    <br>Many hosting providers and ISPs block port 25 as a default practice. If this is the case, contact your host/ISP to find out which ports are open for outgoing SMTP relay. We recommend port 587 to avoid any rate limiting that your server host may apply.
1. Once you successfully connect to the SendGrid, log in to the server by typing `AUTH LOGIN`.
    <br>The mail server responds with `334 VXNlcm5hbWU6`, a Base64 encoded request for your username.
1. Input the API username encoded in Base64. Everyone's username is `apikey`, which is `YXBpa2V5` in Base64.
    <br>The mail server responds with `334 UGFzc3dvcmQ6`. This response is a Base64 encoded request for your password (your API Key).
1. Enter your Base64 converted API key in the next line as the password.
    <br>The mail server responds with `235 Authentication successful`. Getting this far indicates that your connection to smtp.sendgrid.net over the chosen port is open and that your API key is valid.
1. Next, add the email that you’re sending from: `mail from:<<SENDER_EMAIL>>`.
    <br>The mail server responds with `250 Sender address accepted`.
1. Add the email that you’re sending to: `rcpt to:<<RECIEPIENT_ADDRESS>>`.
    <br>The mail server responds with `250 Recipient address accepted`.
1. On the next line, type `DATA` - this indicates that you’re typing the email content.
1. Optionally, add a mail-to header to add the name and email address of the recipient to the email header:
    <br>`To: <<NAME>> <<EMAIL>>`
    <br>Press `[Enter]`
1. Next, add a from header to add the name and email address of the sender to the email header - if not included, SendGrid blocks your email because it doesn’t follow RFC 5322 compliance guidelines:
    <br>`From: <<NAME>> <<EMAIL>`
    <br>Press `[Enter]`
1. Include a subject line:
    <br>`Subject: <<EMAIL_SUBJECT>>`
    <br>Press `[Enter]`
1. Add the content of the message:
    <br>`"<<MESSAGE>>"`. For example: `“This is a test for the SMTP relay."`
    <br>Press `[Enter]`
1. Finally, send the email by typing a period, and then pressing enter:
    <br>`.`
    <br>Press `[Enter]`
    <br>The mail server returns `250 Ok: queued as A1AywHK7T_itbGWaASw2YQ` - This means the email has been queued to send. This queue moves very quickly.
1. Exit the Telnet connection with: `quit`.

Now that you've sent a test email, learn to [integrate your servers with our SMTP service]({{root_url}}/for-developers/sending-email/integrating-with-the-smtp-api/).

<call-out>

Message size limit: The total message size should not exceed 20MB. This includes the message itself, headers, and the combined size of any attachments.

</call-out>


<call-out-link linktext="IMPLEMENTATION SERVICES" img="/img/expert-insights-promo1.png" link="https://sendgrid.com/solutions/email-implementation/">


### Do you want expert help to get your email program started on the right foot?


Save time and feel confident you are set up for long-term success with Email Implementation. Our experts will work as an extension of your team to ensure your email program is correctly set up and delivering value for your business.


</call-out-link>


## Additional Resources

- [Getting Started with the UI]({{root_url}}/ui/sending-email/how-to-send-email-with-marketing-campaigns/)
- [Getting Started with the API]({{root_url}}/for-developers/sending-email/api-getting-started/)
- [SMTP Service Crash Course](https://sendgrid.com/blog/smtp-service-crash-course/)
- [Integrating with the SMTP API]({{root_url}}/for-developers/sending-email/integrating-with-the-smtp-api/)
- [Building an SMTP Email]({{root_url}}/for-developers/sending-email/building-an-x-smtpapi-header/)
