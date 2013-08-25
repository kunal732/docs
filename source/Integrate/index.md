--- layout: page weight: 100 title: Integrate with SendGrid navigation:
show: true --- {% anchor h2 %} Send Email From Your Application {%
endanchor %}

SendGrid provides two ways to send email: through our SMTP relay or
through our [web API]({{root_url}}/API_Reference/Web_API/index.html).

{% info %} We recommend using SMTP in most cases based on the large
number of libraries and documentation available for that protocol. {%
endinfo %} {% anchor h3 %} SMTP {% endanchor %}

SMTP is the fastest way to get started since it requires only minimal
changes:

-   Change your SMTP username and password to your SendGrid credentials
-   Set the server host name to **smtp.sendgrid.net**
-   Use ports 25 or 587 for TLS/plain connections and port 465 for SSL
    connections

{% info %} For most users we suggest **port 587** to avoid rate limits
set by some hosting companies. {% endinfo %} {% info %} With SMTP, 100
messages can be sent with each connection. {% endinfo %}

SendGrid extends SMTP with the X-SMTPAPI header, giving you more control
over how SendGrid sends your email. See the [SMTP
API]({{root_url}}/API_Reference/SMTP_API/index.html) documentation for
more information.

{% info %} Customers should utilize SMTPAPI if this is an option. As
with SMTP, 100 messages can be sent with each connection, but there can
be 1000 recipients for each message. {% endinfo %} {% anchor h3 %} Web
API {% endanchor %}

In some cases the [web
API]({{root_url}}/API_Reference/Web_API/index.html) has advantages over
SMTP:

-   If your ISP blocks all outbound mail ports and your only option is
    HTTP.
-   If there is high latency between your site and ours, the web API
    might be quicker since it does not require as many messages between
    the client and server.
-   If you do not control the application environment and cannot install
    and configure an SMTP library.
-   If you build a library to send email, developing against a web API
    provides quicker development.

{% anchor h2 %} Receive Email From Your Application {% endanchor %}

Though SendGrid does not store messages or provide mailboxes, the
[Inbound Parse Webhook]({{root_url}}/API_Reference/Webhooks/parse.html)
parses the email bodies and attachments from incoming emails and posts
them to your web application.

Examples include posting blog articles via email or processing email
replies.

{% anchor h2 %} Power Users and High Volume Senders {% endanchor %}

A local mail server, such as Postfix, is the most robust way to send
email through SendGrid when configured to queue all email from your
application and then relay the messages through SendGrid as a smart
host. This has the least latency from your application's perspective
with the added benefit of handing your email off to a fault tolerant
server. If internet connectivity between your servers and ours drops, a
local mail server gracefully handles queuing and resending the email, as
opposed to building that intelligence into your sending application.

Local mail servers also have advantages at high volume as they can use
some of the more complex parts of the SMTP protocol, such as connection
reuse and pipelining. With these techniques a mail server sends
significantly more traffic in a given time than if you have individual
scripts connecting for each message.