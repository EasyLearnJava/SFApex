## Welcome to APEX 

## Should we tyecast Trigger.new to get a specific set of objects ?

**Scenario 1:** Trigger.New in Trigger, which works well as it is implicitly type casted to specific object, here is Account.
```
Trigger simpleTrigger on Account (after insert) {
    //works well
    for (Account a : Trigger.new) {
        // Iterate over each Account
    }
}
```

**Scenario 2:** In the Trigger handler class and in a specific method, we are receiving specific list List<Account> as an argument, and trying to loop through the specific object.
```
public class AccountTriggerHandler
{   
    public void onAfterInsert(List<Account> oldList, List<Account> newList)
    {
        //this works fine
        for(Account a:newList)
        {

        }
    }   
}
```

**Scenario 3:** In a method, directly accessing Trigger.new which actually returns list of SObject. So, following code will works well.
```
public class AccountTriggerHandler
{
    public void onAfterInsert()
    {
        //works well
        for(SObject a:Trigger.new)
        {
            if(a instanceOf Account)
            {
                Account accObj = (Account) a;
            }
        }
    }
}
```
    
**Scenario 4:** Type casting Trigger.new to list of Account.
```
public class AccountTriggerHandler
{
    public void onAfterInsert()
    {
        //works well
        for(Account a: (List<Account>)Trigger.new)
        {

        }
    }
}
```
    
**Scenario 5:** Directly accessing Trigger.new without type casting to specific object.
```
public class AccountTriggerHandler
{
    public void onAfterInsert()
    {
        `//will not compile`
        for(Account a: Trigger.new)
        {

        }
    }
}
```    

## Get List of records in SOQL
```
//Using trigger.new directly in IN clause
List<Account> accLst = [select id, name from Account where id in :(Trigger.new)];

//Using Trigger.newMap directly in IN clause    
List<Account> accList = [select id, name from Account where id in :(Trigger.newMap).keyset()];

//Using a custom List in IN Clause
List<Account> accLst = Trigger.new;    
for(Account accRec :accLst){
    if(null != accRec.Business_DUNS_Number__c)
        DunsLst.add(accRec.Business_DUNS_Number__c);
}    
List<Company__c> cLst = [select id, DUNS__c, Account__c from Company__c where DUNS__c in :DUNSLst];    
```    

## Get records to MAP in SOQL
    
    
You can use the [editor on GitHub](https://github.com/EasyLearnJava/SFApex/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

https://jsonplaceholder.typicode.com/
https://www.gitpod.io/
    
# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `RED` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/EasyLearnJava/SFApex/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
