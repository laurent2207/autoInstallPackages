# autoInstallPackages
Function to check whether you have the needed packages installed, if not then the function will install them.

### Explaination of the function's code
The function uses a character vector. This vector will contain the names of the packages which will be used throughout the script. 
The function will start with checking if the packages are installed or not by using the require() function. This function will give FALSE in return if the package is not installen.
```
using <- function(...) {
  libs <- unlist(list(...))
  req <- unlist(lapply(libs, require, character.only=T))
```
If the package is not installed the require() function will return FALSE. The function then will create a character vector which contains these packages. The variable n will store the length of this character vector.  
```
need <- libs[req==F]
  n <- length(need)
```
If n is greater than 0 and is greater than 2, then paste the names of all packages except the last one, else paste only the first packagename.
```
if(n>0){
    libsmsg <- if(n>2) paste(paste(need[1:(n-1)], collapse=", "), ",", sep="") else need[1]
```
If n is greater than 0 and is greater than 1, then paste libsmsg (created in the previous if statement), paste *and* and paste the nth name of the need character vector. 
```
if(n>1){
      libsmsg <- paste(libsmsg," and ", need[n], sep="")
    }
```
So if n would be 2 then n would be between 0 and 2 so n would not be greater than 2. This would return the else statement and thus return the first name in the character vector need. 
However, n would be greater than 1, which will return the variable stored in libsmsg, which in this case is need[1]. It will paste *and* and it will paste need[n], in this case need[2]. So the statement will return need[1] and need[2].

So n is greater than 1, then the following will be returned. A YES/NO messagebox will appear on screen with the message *The following packages could not be found: need[1] and need[2]. Install packages?* 

If the user will choose YES, then the function will use the install.packages() function. The packages that were previously not installed will bed installed now! Additionally, the function will use the require() function again on the newly installed packages.
```
    libsmsg <- paste("The following packages could not be found:\r\r",libsmsg,"\r\rInstall missing packages?", sep="")
    if(winDialog(type = c("yesno"), libsmsg)=="YES"){       
      install.packages(need)
      lapply(need, require, character.only=T)
   }
  }
}
```
**NOTE:** This function will only work with packages available via CRAN!
