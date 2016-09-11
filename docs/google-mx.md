In order for a branch group to receive email through Google Apps for Nonprofits, a DNS MX record must be configured for the branch group's subdomain.

- Log on to [DreamHost](https://dreamhost.com/), where the DNS records for westkingdom.org is managed.
- Click on "Manage DNS" to navigate to the DNS section
- Click on the small "DNS" label below the entry for westkingdom.org (first column of the first row of the table).
- Fill out the entries in the form "Add a custom DNS record to westkingdom.org"
  - Entery branch name in [     ] . westkingdom.org field
  - Set the record type to CNAME
  - Set the value to "westkingdom.org."  Note that the ending dot SHOULD be included.
- Click "Add record now"

It can take four to 48 hours for DNS records to refresh when a change is made. If you complete all of the steps above without ever making a DNS query for the new subdomain (e.g. by typing it into a web browser URL field, etc.), then it is possible that the domain record will be available right away. Wait 60~120 seconds for Dreamhost to finish setting up the record before testing it to avoid caching the "not found" result.

To confirm that the subdomain has been set up, use the `host` program (Mac or Linux):
```
$ host champclair.westkingdom.org
champclair.westkingdom.org is an alias for westkingdom.org.
westkingdom.org has address 50.23.81.227
westkingdom.org mail is handled by 10 ALT4.ASPMX.L.GOOGLE.COM.
westkingdom.org mail is handled by 5 ALT2.ASPMX.L.GOOGLE.COM.
westkingdom.org mail is handled by 10 ALT3.ASPMX.L.GOOGLE.COM.
westkingdom.org mail is handled by 1 ASPMX.L.GOOGLE.COM.
westkingdom.org mail is handled by 5 ALT1.ASPMX.L.GOOGLE.COM.
```
Dreamhost apparantly automatically configures new subdomains of westkingdom.org to have the appropriate Google MX records.  If you do **not** see the google "mail is handled by" sections, first try waiting for a while, and if they don't show up, try checking the MX record administration page on Dreamhost.

Once the DNS records have been set up, it is still necessary to configure Google Apps for Nonprofits.

- Log in to [Google Apps](https://admin.google.com).
- Go to the [Domains](https://admin.google.com/westkingdom.org/AdminHome?hl=en&fral=1#Domains:) section
- Click on “Add a Domain or a Domain Alias”
- Select “Add another domain”
- Enter “BRANCHNAME.westkingdom.org”.
- Click “Continue and verify domain ownership”. It will automatically be verified, since it is a subdomain of westkingdom.org.

That should be all that is necessary; however, if this is a brand-new group that has just been corrected, you may also need to add a new record to the [branch group taxonomy terms](http://www.westkingdom.org/admin/structure/taxonomy/vocabulary_2) on westkingdom.org.

The westkingdom.org user records, combined with the memberships of the officer groups on that site, as managed by the [Regnum](http://westkingdom.org) submission process are what actualy controls the creation of the email lists. Do not use the Google admin pages to create lists or alter memberships, as the automated system will overwrite this information on the next update.

### Determining Branch Groups the Need Configuration

First, you need the groups yaml file:
```
drush @wkweb.stage wkx > westkingdom-groups.yaml
```
Next, run this script (Mac or Linux):
```
(for g in $(grep '^[^ ]' westkingdom-groups.yaml | sed -e 's/:$//' | sort) ; do host "$g.westkingdom.org" ; done) | grep 'mail is handled by' | grep -v GOOGLE | awk '{print $1}' | sort | uniq
```
The result will show any branch group that has officers who have approved Regnum forms, but that have **not** been configured to send email through Google apps.
