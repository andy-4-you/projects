Tutorial for template creation
==============================

A template defines which data attributes you wish to retrieve from an
invoice. Each template should work on all invoices of a company or
subsidiary (e.g. Amazon Germany vs Amazon US).

extract invoice-items using the ``lines``-plugin developed by `Holger
   Brunn <https://github.com/hbrunn>`__

Adding templates is easy and shouldn’t take longer than adding 2-3
invoices by hand. We use a simple YML-format. Many options are optional
and you just need them to take care of edge cases.

Existing templates can be found in the project folder or the installed
package under ``/invoice2data/templates/``. During testing you can use
the ``--template-folder`` option to point to your own, new template
files. If you add or improve templates that could be useful for
everyone, we encourage you to file a pull request to the main repo, so
everyone can use it.

Samples of invoice/bill templates
-----------------------

Here is a sample of a invoice/bill template issued
by TDS Telecommunications (`download 
PDF <https://ecmdesstorage.blob.core.windows.net/test/TDS-16022303-00_Cat.pdf?st=2019-11-07T10%3A45%3A45Z&se=2020-10-27T10%3A45%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=69%2FMb8M51l64UdSaTuZgNL7kaA48mdIG6vv8EVOSNyU%3D>`__):

::

    issuer: TDS Telecommunications LLC
    fields:
    amount: 
        - Total Amount Due\s+\$(\d+\,\d+\.\d+)
        - TOTAL CURRENT CHARGES\s+\$(\d+\.\d+)
        - TOTAL CURRENT CHARGES\s+\$(\d+\,\d+\.\d+)
        - Total Amount Due\s+\$(\d+\.\d+) 
    date: 
        - BILLING DATE\s+(\d{1,2}\W\d{1,2}\W\d{2,4})
    invoice_number:   
        - Invoice Number\W\s+(\d{12})    
    account_id:
        - ACCOUNT NUMBER\s+(\d+\W\d+\W\d+)  
    due_date:
        - Due Date\W\s+(\d{2}\W\d{2}\W\d{2})
    keywords:
    - TDS
    options:
    currency: USD

Extracted text from PDF:


Let’s look at each field:

-  ``issuer``: The name of the invoice issuer. Can have the company name
   and country.
-  ``keywords``: Also a required field. These are used to pick the
   correct template. Be as specific as possible. As we add more
   templates, we need to avoid duplicate matches. Using the VAT number,
   email, website, phone, etc are generally good choices. ALL keywords
   need to match to use the template.
