# The BETYdb API

TODO: What is an API? Why is it useful?

The API has two options: easy and hard. 

## Easy Way

The easy way provides most useful information in a single table in a csv format that is easy to use - in any spreadsheet software or scripting language (or json or xml, see below). 


Data can be downloaded as a `.csv` file, and data from previously published syntheses can be downloaded without login. For example, to download all of the Switchgrass (Panicum virgatum L.) yield data,

    https://www.betydb.org/traits.csv?genus=Switchgrass

## Hard Way 

The "hard way" downloads individual tables from the database. This appraoch allows more complex queries and faster programatic access. In the future, this feature will be used to access BETYdb by the [`rotraits` package](https://github.com/ropensci/rotraits/issues/3) under development by rOpenSci. It would be straightforward to translate funcitons in the PEcAn.DB package to use the API. Currently, PEcAn accesses the PostgreSQL database directly, via ssh tunnel. 

    
Howver, most of the useful information, for example about sites and treatments, is provided on other tables. Thus, useful queries will should  trouble is that this file will need to 

### Cross-table queries using CSV, JSON and XML APIs

BetyDB has the ability to return any object in these three formats. All the tables in BetyDB are RESTful, which allows you to GET, POST, PUT, or DELETE them without interacting with the web interface. Here are some examples. In all of these examples, you can use exchange `.json` and `.xml` depending on the format you want.

The format of your request will need to be:



Return all citations in json format (replace json with xml or csv for those formats)

    https://www.betydb.org/citations.json

Return all yield data for the genus ‘Miscanthus’  
  
    https://www.betydb.org/yields.json?genus=Miscanthus
    https://www.betydb.org/yields.json?genus=Miscanthus&species=giganteus 

Return all yield data from the with author = Heaton and year = 2004 (can also be queried by title)

    https://www.betydb.org/citations.xml?include[]=yields&author=Heaton&year=2004

Find species associated with the `biocro.salix` pft 

    https://www.betydb.org/species.xml?include[]=pfts_species&include[]=pfts&name=biocro.salix

 Return all citations with their associated sites (you use the singular version of the associated tables name - site - when the relationship is many to one, and the plural when many to many; hint: if the id of the table you are attempting to include is in the record - relatedtable_id - then it is the singular version.   

    https://www.betydb.org/citations.json?include[]=sites 

Return all citations with their associated sites and yields (working on ability to nest this deeper)    

    https://www.betydb.org/citations.json?include[]=sites&include[]=yields 

Return all citations with the field journal equal to ‘Agronomy Journal’ and author equal to ‘Adler’ with their associated sites and yields.  

    https://www.betydb.org/citations.json?journal=Agronomy%20Journal&author=Adler&include[]=sites&include[]=yields 

Return citation 1 in json format, can also be achieved by adding ’?id=1’ to line 1  

    https://www.betydb.org/citations/1.json 

Return citation 1 in json format with it’s associated sites  

    https://www.betydb.org/citations/1.json?include[]=sites 

Return citation 1 in json format with it’s associated sites and yields 

    https://www.betydb.org/citations/1.json?include[]=sites&include[]=yields 

## API keys

Using an API key allows access to data without having to enter a login. To use an API key, simply append @?key=@ to the end of the URL. Each user must obtain a unique API key.