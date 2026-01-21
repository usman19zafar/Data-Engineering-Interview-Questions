* When you generate a Sas token, you can assign that to a specific key, So you've got the option to set it to Key1 or Key2. SAS token has been compromised, simply regenerate the access key, the Sas token also becomes invalid.

* There are different types of permissions available to grand, read only etc. for project we selected read and list

* How to generate a SAS token from Azure Storage Explorer, because that's what you might be using in your production environments.

*Go to Storage Explorer, and in order to create a SAS token, simply right click on the container in which you are creating the SAS token and click Get Shared Access Signature.
***The only thing you need to be mindful of is, instead of just the token, you get a query string here which starts with a question mark. So when you copy the query string, just remember to remove the question mark and use it in your notebook.
