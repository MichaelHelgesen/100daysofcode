# R3 Log
## 09.04.19
* I managed to enhance the code for the "Cash Register" challenge. As always, the solution was much more advanced and elegant. But I did solve it though. And I managed to make the code better. My solution:

        function checkCashRegister(price, cash, cid) {
        
        // Find change to return
        var change = cash - price;

        // Copy of the cash in register
        var register = cid;

        // Total used to sum amount of money in register
        var total = 0;

        // Array of what is left in register after returning the change
        var leftInRegister = [];

        // Get total in register
        for (var i = 0; i < register.length; i++) {
            total += Number(register[i][1]);
        }

        // Function for returning change and pushing to new array 
        var changeFunction = function (amount, array) {
            var newChange = change;
            var currency = 0;
            if (change > amount) {
                for (var j = 0; (change >= amount && j < array[1]); j += amount) {
                currency += amount;
                change = (change - amount).toFixed(2);
                }
            leftInRegister.push([array[0], currency]);
            }
        }
        // For loop going through the register
        for (var i = (register.length - 1); i >= 0; i--) {
            if (register[i][0] === "ONE HUNDRED") {
            changeFunction(100, register[i]);
            } else if (register[i][0] === "TWENTY") {
                changeFunction(20, register[i]);
            } else if (register[i][0] === "TEN") {
                changeFunction(10, register[i]);
            } else if (register[i][0] === "FIVE") {
                changeFunction(5, register[i]);
            } else if (register[i][0] === "ONE") {
                changeFunction(1, register[i]);
            } else if (register[i][0] === "QUARTER") {
                changeFunction(0.25, register[i]);
            } else if (register[i][0] === "DIME") {
                changeFunction(0.1, register[i]);
            } else if (register[i][0] === "NICKEL") {
                changeFunction(0.05, register[i]);
            } else if (register[i][0] === "PENNY") {
                changeFunction(0.01, register[i]);
            }
        }

        if ((cash - price) === total) {
            return {
                status: 'CLOSED',
                change: register
            }
            } else if (change != 0) {
            return {
                status: 'INSUFFICIENT_FUNDS',
                change: []
            }
            } else {
            return {
                status: 'OPEN',
                change: leftInRegister 
            }
            }   
        }

        console.log(checkCashRegister(3.26, 100, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.1], ["QUARTER", 4.25], ["ONE", 90], ["FIVE", 55], ["TEN", 20], ["TWENTY", 60], ["ONE HUNDRED", 100]]));

    fCC solution:

        // Create an array of objects which hold the denominations and their values
        var denom = [
        { name: 'ONE HUNDRED', val: 100.00},
        { name: 'TWENTY', val: 20.00},
        { name: 'TEN', val: 10.00},
        { name: 'FIVE', val: 5.00},
        { name: 'ONE', val: 1.00},
        { name: 'QUARTER', val: 0.25},
        { name: 'DIME', val: 0.10},
        { name: 'NICKEL', val: 0.05},
        { name: 'PENNY', val: 0.01}
        ];

        function checkCashRegister(price, cash, cid) {
        var output = { status: null, change: [] };
        var change = cash - price;

        // Transform CID array into drawer object
        var register = cid.reduce(function(acc, curr) {
            acc.total += curr[1];
            acc[curr[0]] = curr[1];
            return acc;
        }, { total: 0 });

        // Handle exact change
        if (register.total === change) {
            output.status = 'CLOSED';
            output.change = cid;
            return output;
        }

        // Handle obvious insufficient funds
        if (register.total < change) {
            output.status = 'INSUFFICIENT_FUNDS';
            return output;
        }

        // Loop through the denomination array
        var change_arr = denom.reduce(function(acc, curr) {
            var value = 0;
            // While there is still money of this type in the drawer
            // And while the denomination is larger than the change remaining
            while (register[curr.name] > 0 && change >= curr.val) {
            change -= curr.val;
            register[curr.name] -= curr.val;
            value += curr.val;

            // Round change to the nearest hundreth deals with precision errors
            change = Math.round(change * 100) / 100;
            }
            // Add this denomination to the output only if any was used.
            if (value > 0) {
                acc.push([ curr.name, value ]);
            }
            return acc; // Return the current change_arr
        }, []); // Initial value of empty array for reduce

        // If there are no elements in change_arr or we have leftover change, return
        // the string "Insufficient Funds"
        if (change_arr.length < 1 || change > 0) {
            output.status = 'INSUFFICIENT_FUNDS';
            return output;
        }

        // Here is your change, ma'am.
        output.status = 'OPEN';
        output.change = change_arr;
        return output;
        }

        // test here
        checkCashRegister(19.50, 20.00, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.10], ["QUARTER", 4.25], ["ONE", 90.00], ["FIVE", 55.00], ["TEN", 20.00], ["TWENTY", 60.00], ["ONE HUNDRED", 100.00]]);
    
    Explanation: First, create an array of objects with the value of each denomination of bill or coin, along with an output object with the status and change keys. Next, transform the CID array into a drawer object. Then, handle the conditions of exact change and insufficient funds. Loop through the denom array and update the change and values while there is still money of each type in the drawer and while the denomination is larger than the change remaining. Add this denomination to the accumulator of change_arr if any of this type was used. After the loop, change_arr is a 2D array of the change due, sorted from highest to lowest denomination. If there are no elements in change_arr or you still owe change, return the output object with a status of INSUFFICIENT_FUNDS. Finally you can give the correct change. Return the output object with a status of OPEN and change_arr as the value of change.

## 08.04.19
* Solved "Cash Register". I spent days on this one. I made it work, but it is a mess. I need to try to make the code shorter and simpler before moving on. There is way to much repeating code. 

## 28.03.19
* Solved "Telephone Number Validator". I was trying and failing a lot with this. I tried out a lot of different things. 

        function telephoneCheck(str) {
        // Good luck!

        let truth = false;

        const regEx = [
            /^[1-9]\d{9}$/g,
            /^(1)[(]\d{3}[)]\d{3}[-]\d{4}$/g,
            /^(1)\s[(]\d{3}[)]\s\d{3}[-]\d{4}$/g,
            /^(1)\s[1-9]{3}[-][1-9]{3}[-][1-9]{4}$/g,
            /^(1)\s[1-9]{3}\s[1-9]{3}\s[1-9]{4}$/g,
            /^[(]\d{3}[)]\d{3}\s\d{4}$/g,
            /^[(]\d{3}[)]\d{3}[-]\d{4}$/g,
            /^[(]\d{3}[)]\s\d{3}[-]\d{4}$/g,
            /^\d{3}[-]\d{3}[-]\d{4}$/g,
            /^\d{3}\s\d{3}\s\d{4}$/g
        ]

        regEx.forEach(function(element, index) {
            if (str.match(element)) {
            truth = true;
            }
        });

        return truth;

        }

        console.log(telephoneCheck("5555555"));

    And of course... the basic solution had it all together...:

        function telephoneCheck(str) {
        var regex = /^(1\s?)?(\(\d{3}\)|\d{3})[\s\-]?\d{3}[\s\-]?\d{4}$/;
        return regex.test(str);
        }
        telephoneCheck("555-555-5555");
    
    Code explanation:
    
    * ^ denotes the beginning of the string.
    * (1\s?)? allows for “1” or “1 ” if there is one.
    * \d{n} checks for exactly n number of digits so \d{3} checks for three digits.
    * x|y checks for either x OR y so (\(\d{3}\)|\d{3}) checks for either three digits surrounded by parentheses, or three digits by themselves with no parentheses.
    * [\s\-]? checks for spaces or dashes between the groups of digits.
    * $ denotes the end of the string. In this case the beginning ^ and end of the string $ are used in the regex to prevent it from matching any longer string that might contain a valid phone number (eg. “s 555 555 5555 3”).
    * Lastly we use regex.test(str) to test if the string adheres to the regular expression, it returns true or false.

## 27.03.19
* Spent the whole evening trying to solve "Telephone Number Validator". Got lost in regex.

## 26.03.19
* Solved "Roman Numeral Converter. I thought about exploring closures on this one, but I could not get my head around it. I searched for a formula to convert number to numerals, and found solutions here: http://blog.functionalfun.net/2009/01/project-euler-89-converting-to-and-from.html. My solution:

        function convertToRoman(num) {
        
        var newNumeral = "";
        var newNum = num;
        var romanNumerals = {
            M : 1000,
            CM: 900,
            D: 500,
            CD: 400,
            C: 100,
            XC: 90,
            L: 50,
            XL: 40,
            X: 10,
            IX : 9,
            V : 5,
            IV : 4,
            I : 1
        }

            Object.keys(romanNumerals).forEach(function(key,index) {
                if((newNum - romanNumerals[key]) >= 0) {
                    for (var i = newNum + 1; i > romanNumerals[key]; i = i - romanNumerals[key]) {
                        newNum = newNum - romanNumerals[key];
                        newNumeral += key
                    }        
                } 
            });

        return newNumeral;
        
        }

        console.log(convertToRoman(3999));
    
    The easy solution is of course simpler. I completely forgot about the while loop...:

        var convertToRoman = function(num) {

        var decimalValue = [ 1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1 ];
        var romanNumeral = [ 'M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I' ];

        var romanized = '';

        for (var index = 0; index < decimalValue.length; index++) {
            while (decimalValue[index] <= num) {
            romanized += romanNumeral[index];
            num -= decimalValue[index];
            }
        }

        return romanized;
        }

        // test here
        convertToRoman(36);

    The two intermediate ones where difficult for me to understand.

    Intermediate 1:

        function convertToRoman(num) {
        var romans = ["I", "V", "X", "L", "C", "D", "M"],
            ints = [],
            romanNumber = [],
            numeral = "";
        while (num) {
            ints.push(num % 10);
            num = Math.floor(num/10);
        }
        for (i=0; i<ints.length; i++){
            units(ints[i]);
        }
        function units(){
            numeral = romans[i*2];
            switch(ints[i]) {
            case 1:
                romanNumber.push(numeral);
                break;
            case 2:
                romanNumber.push(numeral.concat(numeral));
                break;
            case 3:
                romanNumber.push(numeral.concat(numeral).concat(numeral));
                break;
            case 4:
                romanNumber.push(numeral.concat(romans[(i*2)+1]));
                break;
            case 5:
                romanNumber.push(romans[(i*2)+1]);
                break;
            case 6:
                romanNumber.push(romans[(i*2)+1].concat(numeral));
                break;
            case 7:
                romanNumber.push(romans[(i*2)+1].concat(numeral).concat(numeral));
                break;
            case 8:
                romanNumber.push(romans[(i*2)+1].concat(numeral).concat(numeral).concat(numeral));
                break;
            case 9:
                romanNumber.push(romans[i*2].concat(romans[(i*2)+2]));
            }
            }
        return romanNumber.reverse().join("").toString();
        }

        // test here
        convertToRoman(97);

    Intermediate 2:

        function convertToRoman(num) {
        var romans = 
        // 10^i 10^i*5
            ["I", "V"], // 10^0
            ["X", "L"], // 10^1
            ["C", "D"], // 10^2
            ["M"]       // 10^3
        ],
            digits = num.toString()
                .split('')
                .reverse()
                .map(function(item, index) {
                return parseInt(item);
                }),
            numeral = "";

        // Loop through each digit, starting with the ones place
        for (var i=0; i<digits.length; i++) {
            // Make a Roman numeral that ignores 5-multiples and shortening rules
            numeral = romans[i][0].repeat(digits[i]) + numeral;
            // Check for a Roman numeral 5-multiple version
            if (romans[i][1]) {
            numeral = numeral
                // Change occurrences of 5 * 10^i to the corresponding 5-multiple Roman numeral
                .replace(romans[i][0].repeat(5), romans[i][1])
                // Shorten occurrences of 9 * 10^i
                .replace(romans[i][1] + romans[i][0].repeat(4), romans[i][0] + romans[i+1][0])
                // Shorten occurrences of 4 * 10^i
                .replace(romans[i][0].repeat(4), romans[i][0] + romans[i][1]);
            }
        }

        return numeral;
        }

        // test here
        convertToRoman(36);
    
    But after seeing the while loop, I changed my code:

        function convertToRoman(num) {
        
        var newNumeral = "";
        var newNum = num;
        var romanNumerals = {
            M : 1000,
            CM: 900,
            D: 500,
            CD: 400,
            C: 100,
            XC: 90,
            L: 50,
            XL: 40,
            X: 10,
            IX : 9,
            V : 5,
            IV : 4,
            I : 1
        }

            Object.keys(romanNumerals).forEach(function(key,index) {
                while (newNum - romanNumerals[key] >= 0) {
                    newNum -= romanNumerals[key];
                    newNumeral += key
                }
            });

        return newNumeral;

        }

        console.log(convertToRoman(3999));

* Finished Caesars Cipher. My solution:  

        function rot13(str) { // LBH QVQ VG!
        
        var newWord = "";

        for (var i = 0; i < str.length; i++) {
            
            if (str.charCodeAt(i) >= 65 && str.charCodeAt(i) <= 77) {
                newWord += String.fromCharCode(str.charCodeAt(i) + 13);
            } else if (str.charCodeAt(i) >= 78 && str.charCodeAt(i) <= 90) {
                newWord += String.fromCharCode(str.charCodeAt(i) - 13);
            } else {
                newWord += String.fromCharCode(str.charCodeAt(i));  
            }
        }    

        return newWord;
        }

        // Change the inputs below to test
        console.log(rot13("LBH QVQ VG!"));

    This is the fCC solution. Of course chaining and using map. I need to start to do more advanced solutions:

            function rot13(str) {
            // Split str into a character array
            return str.split('')
            // Iterate over each character in the array
                .map.call(str, function(char) {
                // Convert char to a character code
                x = char.charCodeAt(0);
                // Checks if character lies between A-Z
                if (x < 65 || x > 90) {
                    return String.fromCharCode(x);  // Return un-converted character
                }
                //N = ASCII 78, if the character code is less than 78, shift forward 13 places
                else if (x < 78) {
                    return String.fromCharCode(x + 13);
                }
                // Otherwise shift the character 13 places backward
                return String.fromCharCode(x - 13);
                }).join('');  // Rejoin the array into a string
            }

    Here is also a clever one:

            // Solution with Regular expression and Array of ASCII character codes
            function rot13(str) {
            var rotCharArray = [];
            var regEx = /[A-Z]/ ;
            str = str.split("");
            for (var x in str) {
                if (regEx.test(str[x])) {
                // A more general approach
                // possible because of modular arithmetic
                // and cyclic nature of rot13 transform
                rotCharArray.push((str[x].charCodeAt() - 65 + 13) % 26 + 65);
                } else {
                rotCharArray.push(str[x].charCodeAt());
                }
            }
            str = String.fromCharCode.apply(String, rotCharArray);
            return str;
            }

            // Change the inputs below to test
            rot13("LBH QVQ VG!");

        ALPHA	KEY	BASE 	 	 	 ROTATED	ROT13
        -------------------------------------------------------------
        [A]     65  <=>   0 + 13  =>  13 % 26  <=>  13 + 65 = 78 [N]
        [B]     66  <=>   1 + 13  =>  14 % 26  <=>  14 + 65 = 79 [O]
        [C]     67  <=>   2 + 13  =>  15 % 26  <=>  15 + 65 = 80 [P]
        [D]     68  <=>   3 + 13  =>  16 % 26  <=>  16 + 65 = 81 [Q]
        [E]     69  <=>   4 + 13  =>  17 % 26  <=>  17 + 65 = 82 [R]
        [F]     70  <=>   5 + 13  =>  18 % 26  <=>  18 + 65 = 83 [S]
        [G]     71  <=>   6 + 13  =>  19 % 26  <=>  19 + 65 = 84 [T]
        [H]     72  <=>   7 + 13  =>  20 % 26  <=>  20 + 65 = 85 [U]
        [I]     73  <=>   8 + 13  =>  21 % 26  <=>  21 + 65 = 86 [V]
        [J]     74  <=>   9 + 13  =>  22 % 26  <=>  22 + 65 = 87 [W]
        [K]     75  <=>  10 + 13  =>  23 % 26  <=>  23 + 65 = 88 [X]
        [L]     76  <=>  11 + 13  =>  24 % 26  <=>  24 + 65 = 89 [Y]
        [M]     77  <=>  12 + 13  =>  25 % 26  <=>  25 + 65 = 90 [Z]
        [N]     78  <=>  13 + 13  =>  26 % 26  <=>   0 + 65 = 65 [A]
        [O]     79  <=>  14 + 13  =>  27 % 26  <=>   1 + 65 = 66 [B]
        [P]     80  <=>  15 + 13  =>  28 % 26  <=>   2 + 65 = 67 [C]
        [Q]     81  <=>  16 + 13  =>  29 % 26  <=>   3 + 65 = 68 [D]
        [R]     82  <=>  17 + 13  =>  30 % 26  <=>   4 + 65 = 69 [E]
        [S]     83  <=>  18 + 13  =>  31 % 26  <=>   5 + 65 = 70 [F]
        [T]     84  <=>  19 + 13  =>  32 % 26  <=>   6 + 65 = 71 [G]
        [U]     85  <=>  20 + 13  =>  33 % 26  <=>   7 + 65 = 72 [H]
        [V]     86  <=>  21 + 13  =>  34 % 26  <=>   8 + 65 = 73 [I]
        [W]     87  <=>  22 + 13  =>  35 % 26  <=>   9 + 65 = 74 [J]
        [X]     88  <=>  23 + 13  =>  36 % 26  <=>  10 + 65 = 75 [K]
        [Y]     89  <=>  24 + 13  =>  37 % 26  <=>  11 + 65 = 76 [L]
        [Z]     90  <=>  25 + 13  =>  38 % 26  <=>  12 + 65 = 77 [M]

## 25.03.19
* Tried to solve Roman Numeral Converter. Almost had it.

## 24.03.19
* Finished "Arguments Optional". My solution:

        function addTogether(...args) {

        var sum = 0;

        for (var i = 0; i < args.length; i++) {
            if (typeof args[i] == "number") {
            sum += args[i];
            } else return undefined;
        }

        if (args.length < 2) {
            return displayArgs;
        }

        // The function to return
        function displayArgs(num) {
            if (typeof num != "number") {
            return undefined
            }
            return (Number(num) + Number(args));
        }
        
        return sum;

        }

        addTogether(2,3);

    I realized I had code repeating here, so I wanted to create a function to check if an argument was a number:

        function addTogether(...args) {

        var sum = 0;

        var checkNum = function(num) {
            if (typeof num !== 'number') {
            return undefined;
            } else
            return num;
            };

        for (var i = 0; i < args.length; i++) {
            console.log(checkNum(args[i]);
            sum += args[i];
        }

        if (args.length < 2) {
            return displayArgs;
        }

        // The function to return
        function displayArgs(num) {
            if (typeof num != "number") {
            return undefined
            }
            return (Number(num) + Number(args));
        }
        
        return sum;

        }

        addTogether(2,[3]);

    This did not work. Because if the second argument was not a number, the if else statement would run one time before returning undefined, so it would haver something in the variable "sum" to return. So I had to find a way to work around that. This is the result, after also cleaning it up a bit:

        function addTogether(...args) {

        var checkNum = function(num) {
            if (typeof num != "number") {
            return undefined;
            } else
            return num;
            };

        if (args.length < 2 && checkNum(args[0])) {
            return function (num) {
            if (checkNum(num)) {
            return num + args[0];
            } else {
            return undefined
            }
            }
        } else {
            var sum = 0;
            for (var i = 0; i < args.length; i++) {
            if(checkNum(args[i])) {
            sum += args[i];
            } else {
            return undefined
            };
        }
        return sum;
        }
        }

        addTogether(2, 3);

* Finished "Make a Person". My solution:

        var Person = function(firstAndLast) {

        var newArray = firstAndLast.split(" ");
        var firstName = newArray[0];
        var lastName = newArray[1];

        this.getFirstName = function() {
            return firstName;
        },
        this.getLastName = function() {
            return lastName;
        },
        this.getFullName = function() {
            return firstName + " " + lastName;
        },
        this.setFirstName = function(first) {
            firstName = first;
            return firstName;
        },
        this.setLastName = function(last) {
            lastName = last;
            return lastName;
        },
        this.setFullName = function(firstAndLast) {
            newArray = firstAndLast.split(" ");
            firstName = newArray[0];
            lastName = newArray[1];
        }
        };

        var bob = new Person('Bob Ross');
    
    fCC solution:

        var Person = function(firstAndLast) {
        var fullName = firstAndLast;

        this.getFirstName = function() {
            return fullName.split(" ")[0];
        };

        this.getLastName = function() {
            return fullName.split(" ")[1];
        };

        this.getFullName = function() {
            return fullName;
        };

        this.setFirstName = function(name) {
            fullName = name + " " + fullName.split(" ")[1];
        };

        this.setLastName = function(name) {
            fullName = fullName.split(" ")[0] + " " + name;
        };

        this.setFullName = function(name) {
            fullName = name;
        };
        };

        var bob = new Person('Bob Ross');
        bob.getFullName();

* Finished "Map the Debris". My solution:

        function orbitalPeriod(arr) {
        var GM = 398600.4418;
        var earthRadius = 6367.4447
        
        var a = 2 * Math.PI;
        var b = 0;
        var c = 0;

        for (var i = 0; i < arr.length; i++) {
            b = Math.pow(earthRadius + arr[i].avgAlt, 3);
            c = Math.sqrt(b / GM);
            delete arr[i].avgAlt;
            arr[i].orbitalPeriod = Math.round(a * c);
        }

        return arr;  
        }

        console.log(orbitalPeriod([{name: "iss", avgAlt: 413.6}, {name: "hubble", avgAlt: 556.7}, {name: "moon", avgAlt: 378632.553}]));

    fCC solution: 

        function orbitalPeriod(arr) {
        var GM = 398600.4418;
        var earthRadius = 6367.4447;
        var a = 2 * Math.PI;
        var newArr = [];
        var getOrbPeriod = function(obj) {
            var c = Math.pow(earthRadius + obj.avgAlt, 3);
            var b = Math.sqrt(c / GM);
            var orbPeriod = Math.round(a * b);
            delete obj.avgAlt;
            obj.orbitalPeriod = orbPeriod;
            return obj;
        };

        for (var elem in arr) {
            newArr.push(getOrbPeriod(arr[elem]));
        }

        return newArr;
        }

    I guess this one is better because it doesn't alter the arguments and returns a new array. But the intermediate solution is the same way as mine: 

        function orbitalPeriod(arr) {
        var GM = 398600.4418;
        var earthRadius = 6367.4447;

        //Looping through each key in arr object
        for(var prop in arr) {
            //Rounding off the orbital period value
            var orbitalPer = Math.round(2 * Math.PI * Math.sqrt(Math.pow(arr[prop].avgAlt + earthRadius, 3) / GM));
            //deleting the avgAlt property
            delete arr[prop].avgAlt;
            //adding orbitalPeriod property
            arr[prop].orbitalPeriod = orbitalPer;
        }

        return arr;
        }

* Finished Palindrome Checker. My solution:

        function palindrome(str) {
        var word = str.replace(/[^A-Za-z0-9]/g,'').toLowerCase().split("");
        var length = word.length;

        for (var i = 0; i < word.length; i++) {
            length--
            if (word[i] != word[length]) {
            return false;
            } 
        }

        return true;
        }

        console.log(palindrome("_eye"));

    The basic solution was much more simple: 

            function palindrome(str) {
            return str.replace(/[\W_]/g, '').toLowerCase() ===
                    str.replace(/[\W_]/g, '').toLowerCase().split('').reverse().join('');
            }
    
            But the advanced solution had important notes. It's a much more effective solution:

            //this solution performs at minimum 7x better, at maximum infinitely better.
            //read the explanation for the reason why. I just failed this in an interview.
            function palindrome(str) {
            //assign a front and a back pointer
            let front = 0
            let back = str.length - 1

            //back and front pointers won't always meet in the middle, so use (back > front)
            while (back > front) {
                //increments front pointer if current character doesn't meet criteria
                if ( str[front].match(/[\W_]/) ) {
                front++
                continue
                }
                //decrements back pointer if current character doesn't meet criteria
                if ( str[back].match(/[\W_]/) ) {
                back--
                continue
                }
                //finally does the comparison on the current character
                if ( str[front].toLowerCase() !== str[back].toLowerCase() ) return false
                front++
                back--
            }

            //if the whole string has been compared without returning false, it's a palindrome!
            return true

            }

## D32 #100DaysOfCode - 21.12.18
* Finished "everything be true". My solution: 

        function truthCheck(collection, pre) {
        
        let truth = true;

        for (let i = 0; i < collection.length; i++) {
            if (!collection[i].hasOwnProperty(pre) || !collection[i][pre]) {
            truth = false;
            }
        }
        return truth;
        
        }

        console.log(truthCheck([{"user": "Tinky-Winky", "sex": "male"}, {"user": "Dipsy", "sex": "male"}, {"user": "Laa-Laa", "sex": "female"}, {"user": "Po", "sex": "female"}], "sex"));
    
    fCC intermediate solution (i forgot about "every"):

        function truthCheck(collection, pre) {
        return collection.every(function (element) {
            return element.hasOwnProperty(pre) && Boolean(element[pre]);
        });
        }


## D31 #100DaysOfCode - 20.12.18
* Finished "Binary Agents". I have a feeling my code is really simple, but right now I cant wrap my head around a more elegant solution.

        function binaryAgent(str) {
        //return str;


        let newArr = str.split(' ');

        for (let i = 0; i < newArr.length; i++) {
        newArr[i] = newArr[i].split('');
        let num = 0;
        let add = 128;
            for (let j = 1; j < newArr[i].length; j++) {
            add /=  2;
            if (newArr[i][j] == 1) {
                num += add;
            }  
            }
            newArr[i] = String.fromCharCode(num);
        }

        return newArr.join('');

        }

        console.log(binaryAgent("01000001 01110010 01100101 01101110 00100111 01110100 00100000 01100010 01101111 01101110 01100110 01101001 01110010 01100101 01110011 00100000 01100110 01110101 01101110 00100001 00111111"));

Apparently we can use the parseInt to convert the binary to decimal. fCC solution basic: 

        function binaryAgent(str) {
        biString = str.split(' ');
        uniString = [];

        /*using the radix (or base) parameter in parseInt, we can convert the binary
        number to a decimal number while simultaneously converting to a char*/

        for(i=0;i < biString.length;i++){
            uniString.push(String.fromCharCode(parseInt(biString[i], 2)));
        }

        // we then simply join the string
        return uniString.join('');
        }

fCC intermediate. A bit more like mine: 

        function binaryAgent(str) {
            // Separate the binary code by space.
            str = str.split(' ');
            var power;
            var decValue = 0;
            var sentence = '';

            // Check each binary number from the array.
            for (var s = 0; s < str.length; s++) {
                // Check each bit from binary number
                for (var t = 0; t < str[s].length; t++) {
                // This only takes into consideration the active ones.
                if (str[s][t] == 1) {
                    // This is quivalent to 2 ** position
                    power = Math.pow(2, +str[s].length - t - 1);
                    decValue += power;

                    // Record the decimal value by adding the number to the previous one.
                }
                }

                // After the binary number is converted to decimal, convert it to string and store
                sentence += (String.fromCharCode(decValue));

                // Reset decimal value for next binary number.
                decValue = 0;
            }

            return sentence;
            }

And finaly the elegant advanced method:

        function binaryAgent(str) {
        return String.fromCharCode(...str.split(" ").map(function(char){ return parseInt(char, 2); }));
        }

## D30 #100DaysOfCode - 19.12.18

### D29 #100DaysOfCode - 12.12.18

### D28 #100DaysOfCode - 11.12.18
* Finished "Steamroller", but could not solve it. I had to look at the solution. 

        function steamrollArray(arr) {
        var flattenedArray = [];

        // Create function that adds an element if it is not an array.
        // If it is an array, then loops through it and uses recursion on that array.
        var flatten = function(arg) {
            if (!Array.isArray(arg)) {
            flattenedArray.push(arg);
            } else {
            for (var a in arg) {
                flatten(arg[a]);
            }
            }
        };

        // Call the function for each element in the array
        arr.forEach(flatten);
        return flattenedArray;
        }

        // test here
        steamrollArray([1, [2], [3, [[4]]]]);

Intermediate: 

        function steamrollArray(arr) {
        let flat = [].concat(...arr);
        return flat.some(Array.isArray) ? steamrollArray(flat) : flat;
        }

        flattenArray([1, [2], [3, [[4]]]]);

### D27 #100DaysOfCode - 10.12.18
* Worked on "Steamroller" challenge. 

### D26 #100DaysOfCode - 09.12.18
* Read about last challenge and asked in the forums about it. 

### D025 #100DaysOfCode - 08.12.18
Finished "Drop it", but I am not sure as to some of them are working. Solution 1: 

        let count = 0;

        for (let i = 0; i < arr.length; i++) {
        console.log(i);
        if (func(arr[i])) {
            count = i;
            break
        } else {
            count = arr.length;
        }
        }
        return (arr.slice(count))

Solution 2. I am not sure how this is working. I am not putting any number as argument to the function: 

        if (arr.findIndex(func) < 0) {
            return [];
        } else {return arr.slice(arr.findIndex(func))};

Solution 3. Same here. I am curious as to how this works: 

        while (arr.findIndex(func) ) {
        arr.shift();
        } 
        return arr

fCC solution basic:

        function dropElements(arr, func) {
        // drop them elements.
        var times = arr.length;
        for (var i = 0; i < times; i++) {
            if (func(arr[0])) {
            break;
            } else {
            arr.shift();
            }
        }
        return arr;
        }

        // test here
        dropElements([1, 2, 3, 4], function(n) {return n >= 3;})

Intermediate:

        function dropElements(arr, func) {
        return arr.slice(arr.findIndex(func) >= 0 ? arr.findIndex(func): arr.length, arr.length);
        }

        // test here
        dropElements([1, 2, 3, 4], function(n) {return n >= 3;});

Advanced: 

        function dropElements(arr, func) {
        while(arr.length > 0 && !func(arr[0])) {
            arr.shift();
        }
        return arr;
        }

        // test here
        dropElements([1, 2, 3, 4], function(n) {return n >= 3;});

### D024 #100DaysOfCode - 07.12.18
Finished 

### D023 #100DaysOfCode - 06.12.18
When the alarm woke me today, I suddenly came up with an idea how to solve it! That was a wonderful feeling. I love when that happens. 

My solution: 

        function sumFibs(num) {
        
        let count = 0;
        let stored = 1;
        let oddFibonacci = 1;
        
        for (let i = 0; i < num; i++) {
        let sum = count + stored;
        if ((sum % 2) > 0 && sum <= num) {
            oddFibonacci += sum;
        }
        count = stored;
        stored = sum;
        }
        
        return oddFibonacci;
        }
        sumFibs(75025);

fCC solution: 

        function sumFibs(num) {
            var prevNumber = 0;
            var currNumber = 1;
            var result = 0;
            while (currNumber <= num) {
                if (currNumber % 2 !== 0) {
                    result += currNumber;
                }

                currNumber += prevNumber;
                prevNumber = currNumber - prevNumber;
            }

            return result;
        }

* I finished "Sum All Primes". I have difficulty explaining the code, and I am sure that it is a much better and easier way to do it. But math is not my strong side, and I need more practice doing these kinds of problems. My solution: 

        function sumPrimes(num) {
        
        let sumAllPrimes = 0;

        for (let i = 2; i <= num; i++) {
            
            let prime = true;

            for (let j = i-1; j > 1; j--) {
            if (Number.isInteger( i / j)) {
                prime = false;
            }
            }

        if (prime) {
            sumAllPrimes += i;
        };
        };
        
        console.log(sumAllPrimes);

        return sumAllPrimes;
        }

        sumPrimes(977);

    fCC solution basic:

        function sumPrimes(num) {
        var res = 0;

        // Function to get the primes up to max in an array
        function getPrimes(max) {
            var sieve = [];
            var i;
            var j;
            var primes = [];
            for (i = 2; i <= max; ++i) {
            if (!sieve[i]) {
                // i has not been marked -- it is prime
                primes.push(i);
                for (j = i << 1; j <= max; j += i) {
                sieve[j] = true;
                }
            }
            }

            return primes;
        }

        // Add the primes
        var primes = getPrimes(num);
        for (var p = 0; p < primes.length; p++) {
            res += primes[p];
        }

        return res;
        }

        // test here
        sumPrimes(10);

    fCC solution intermediate:

        function sumPrimes(num) {
        // function to check if the number presented is prime
        function isPrime(number){
            for (i = 2; i <= number; i++){
                if(number % i === 0 && number!= i){
                // return true if it is divisible by any number that is not itself.
                    return false;
                }
            }
            // if it passes the for loops conditions it is a prime
            return true;
        }
        // 1 is not a prime, so return nothing, also stops the recursive calls.
        if (num === 1){
            return 0;
        }
        // Check if your number is not prime
        if(isPrime(num) === false){
        // for non primes check the next number down from your maximum number, do not add anything to your answer
            return sumPrimes(num - 1);
        }

        // Check if your number is prime
        if(isPrime(num) === true){
        // for primes add that number to the next number in the sequence through a recursive call to our sumPrimes function.
            return num + sumPrimes(num - 1);
        }
        }
        // test here
        sumPrimes(10); 

### D022 #100DaysOfCode - 05.12.18
* Worked on "Sum All Odd Fibonacci Numbers". I struggled with the logic. Just couldn't get my head around it.

### D021 #100DaysOfCode - 04.12.18
* Finished Convert HTML Entities. Pretty proud of beeing able to solve it so fast. I have no idea if it's a good solution though: 

        function convertHTML(str) {
        
        const charactersToConvert = {
            34: '&quot;',
            38: '&amp;',
            39: '&apos;',
            60: '&lt;',
            62: '&gt;'
        }

        const regex = /\&|\<|\>|\"|\'/g
        
        return str.replace(regex, function(match, offset, string) {
            return charactersToConvert[match.charCodeAt(match)]
        });
        }

        console.log(convertHTML("<>"));
    
    fCC solution: basic, using a switch:

            var temp = str.split('');

            // Since we are only checking for a few HTML elements I used a switch

            for (var i = 0; i < temp.length; i++) {
                switch (temp[i]) {
                case '<':
                    temp[i] = '&lt;';
                    break;
                case '&':
                    temp[i] = '&amp;';
                    break;
                case '>':
                    temp[i] = '&gt;';
                    break;
                case '"':
                    temp[i] = '&quot;';
                    break;
                case "'":
                    temp[i] = "&apos;";
                    break;
                }
            }

            temp = temp.join('');
            return temp;
    
    fCC Advanced:

        htmlEntities={
            '&':'&amp;',
            '<':'&lt;',
            '>':'&gt;',
            '"':'&quot;',
            '\'':"&apos;"
        };
        //Use map function to return a filtered str with all entities changed automatically.
        return str.split('').map(entity => htmlEntities[entity] || entity).join('');
        }

    Seeing the above code, I changed mine a bit, since I had unnecessary code:

        function convertHTML(str) {

        const charactersToConvert = {
            '&':'&amp;',
            '<':'&lt;',
            '>':'&gt;',
            '"':'&quot;',
            '\'':"&apos;"
        }

        const regex = /\&|\<|\>|\"|\'/g;
        
        return str.replace(regex, function(match) {
            return charactersToConvert[match]
        });
        }

        console.log(convertHTML("<>"));
    


### D020 #100DaysOfCode - 03.12.18
* I shortened the code a bit. It was the same as the intermediate solution on fCC: 

        function fearNotLetter(str) {
        for (let i = 0; i < str.length; i++) {
            if ((str.charCodeAt(i+1) - str.charCodeAt(i)) > 1) {
            return String.fromCharCode(str.charCodeAt(i) +1);
            } 
        }
        }
        fearNotLetter("abcdefghjklmno");

    fCC solution: 

        // Adding this solution for the sake of avoiding using 'for' and 'while' loops.
        // See the explanation for reference as to why. It's worth the effort.

        function fearNotLetter(str) {
        var compare = str.charCodeAt(0), missing;

        str.split('').map(function(letter,index) {
            if (str.charCodeAt(index) == compare) {
            ++compare;
            } else {
            missing = String.fromCharCode(compare);
            }
        });

        return missing;
        }

        // test here
        fearNotLetter("abce");

* Finished "Sorted Union". I wanted to use reduce and I made it! It made me very happy. My solution: 

        function uniteUnique(...arr) {

        return arr.reduce(function(prev, next, index, array) {
            return prev.concat(
            next.filter(function(item) {
                return !prev.includes(item);
            })
            );
        });
        };
        uniteUnique([1, 2, 3], [5, 2, 1, 4], [2, 1], [6, 7, 8]);
    
    Basic fCC solution (not so basic to me :D)

        function uniteUnique(arr1, arr2, arr3) {
        // Creates an empty array to store our final result.
        var finalArray = [];

        // Loop through the arguments object to truly made the program work with two or more arrays
        // instead of 3.
        for (var i = 0; i < arguments.length; i++) {
            var arrayArguments = arguments[i];

            // Loops through the array at hand
            for (var j = 0; j < arrayArguments.length; j++) {
            var indexValue = arrayArguments[j];

            // Checks if the value is already on the final array.
            if (finalArray.indexOf(indexValue) < 0) {
                finalArray.push(indexValue);
            }
            }
        }

        return finalArray;
        }

        // test here
        uniteUnique([1, 3, 2], [5, 2, 1, 4], [2, 1]);

### D019 #100DaysOfCode - 02.12.18
* Finished "Missing Letters. My solution: 

        function fearNotLetter(str) {
        
        let newArr = str.split('');
        let missingLetter = '?'

        for (let i = 0; i < newArr.length; i++) {
            if ((str.charCodeAt(i+1) - str.charCodeAt(i)) > 1) {
            return missingLetter = String.fromCharCode(str.charCodeAt(i) +1);
            } else missingLetter = undefined;
        }
        return missingLetter;
        }
        fearNotLetter("abcdefghjklmno");

    But I think I can do this another way. So I want to experiment a bit before I check the solution. 

### D018 #100DaysOfCode - 01.12.18
* Finished "DNA Pairing". My solution:

        function pairElement(str) {
        
        let newArr = str.split('');
        let DNAarr = [];

        let DNAobject = {
            pair1: 'TA',
            pair2: 'CG'
        }
        
        for (let i= 0; i < newArr.length; i++) {

            let pairArray = [];
            
            pairArray.push(str[i]);

            for (let value of Object.values(DNAobject)) {
            if (value.includes(str.charAt(i))) {
                pairArray.push(value.charAt(!value.indexOf(str.charAt(i))));
            };
        };
        DNAarr.push(pairArray);
        };
        
        return DNAarr;
        }
        pairElement("ATCGA");

    fCC used switch and map for the task. The map one was simplest: 

            function pairElement(str) {
            //create object for pair lookup
            var pairs = {
            "A": "T",
            "T": "A",
            "C": "G",
            "G": "C"
            }
            //split string into array of characters
            var arr = str.split("");
            //map character to array of character and matching pair
            return arr.map(x => [x,pairs[x]]);
        }

        //test here
        pairElement("GCG");

### D017 #100DaysOfCode - 30.11.18
* Finished "Pig Latin". Spent way to many hours trying to figure out a way to do it with one line. But I didn't make it, so I switched to a if/else. Again, I approach these challenges more complicated than I need to. I think it's better if I solve them the best way I can, and than spend time trying to make it better or studying the fCC solutions. 

    My solution: 

        function translatePigLatin(str) {
        
        let newArr = [];
        let regex = /[aeiou]+/gi
        
        if (str[0].match(regex)) {
            newArr = str + 'way'
        } else {
            let consonant = /^[^aeiou]+/;
            newArr = str.replace(consonant, '').concat(str.match(consonant)) + 'ay';
        } 
        return newArr;
        }
        console.log(translatePigLatin("california"));


    fCC solution 1: 

        function translatePigLatin(str) {
        // Create variables to be used
        var pigLatin = '';
        var regex = /[aeiou]/gi;

        // Check if the first character is a vowel
        if (str[0].match(regex)) {
            pigLatin = str + 'way';

        } else if(str.match(regex) === null) {
            // Check if the string contains only consonants
            pigLatin = str + 'ay';
        } else {

            // Find how many consonants before the first vowel.
            var vowelIndice = str.indexOf(str.match(regex)[0]);

            // Take the string from the first vowel to the last char
            // then add the consonants that were previously omitted and add the ending.
            pigLatin = str.substr(vowelIndice) + str.substr(0, vowelIndice) + 'ay';
        }

        return pigLatin;
        }

        // test here
        translatePigLatin("consonant");

* Finished Intermediate Algorithm Scripting "Search and Replace". My solution: 

        function myReplace(str, before, after) {
        
        let regex = (/[A-Z]/)

        if (before[0].match(regex)) {
            after = after.replace(/^[a-z]/, after[0].toUpperCase());
        }   
        return str.replace(before, after);
        }
        console.log(myReplace("Let us get back to more Coding", "back", "Algorithms"));
    
    fCC solution:

        function myReplace(str, before, after) {
        // Find index where before is on string
        var index = str.indexOf(before);
        // Check to see if the first letter is uppercase or not
        if (str[index] === str[index].toUpperCase()) {
            // Change the after word to be capitalized before we use it.
            after = after.charAt(0).toUpperCase() + after.slice(1);
        }
        // Now replace the original str with the edited one.
        str = str.replace(before, after);

        return str;
        }

        // test here
        myReplace("A quick brown fox jumped over the lazy dog", "jumped", "leaped");

### D016 #100DaysOfCode - 29.11.18
* Did Intermediate Algorithm Scripting: "Wherefore art thou". I did not solve this. I tried a lot of different ways. I did however learn a lot about object keys and properties.  I had to find a solution and copy it and modify it to make it work. I did not understand the return values. I have to remember this. A return in a wrong place or not at all breaks the code easily. When mixing filter, for loops it's easy to misplace it. 

This is the solution: 

        function whatIsInAName(collection, source) {
        // What's in a name?
        var arr = [];
        // Only change code below this line

        // Get the keys we want to match
        let sourceKeys = Object.keys(source);

        // Create a value
        let result;

        // Filter the "collection". This creates a new array.
        arr = collection.filter(function(item) {
            // For every object in the "collection" array, we need to loop through it to match it up to the "source" object.
            // We have already collected the keys from "source" so we know how many key value pairs it contains.
            for(var i = 0; i < sourceKeys.length; i++) {
            // For every object in the "collection" array, we check to see if it has the same property and the same property value. 
            if(item.hasOwnProperty(sourceKeys[i]) && item[sourceKeys[i]] == source[sourceKeys[i]]) {
                // If it has, we set "result" to true.
                result = true;
            } else {
                // If it's not the same, we need to break out of the loop. 
                return false;
            }
            }
            // Then we return the object to the filter, which again returns it to the array. 
            return result;
        });
        console.log(arr)

        // Only change code above this line
        return arr;
        }

        whatIsInAName([{ "apple": 1, "bat": 2 }, { "apple": 1 }, { "apple": 1, "bat": 2, "cookie": 2 }, { "bat":2 }], { "apple": 1, "bat": 2 });

* Finished Intermediate Algorithm Scripting: Spinal Tap Case. My solutions:

        return str.split(/(?=[A-Z])/).join(' ').replace(/\s+/g, '-').replace(/\_/g, '').toLowerCase();
    
    Solution 2: 

        return str.split(/((?=[A-Z])|\-|\_|\s)/).filter(function(item) {
        return (item && !item.includes('_') && !item.includes(' ') && !item.includes('-'));
        }).join('-').toLowerCase()

    fCC basic solution: 

        // Create a variable for the white space and underscores.
        var regex = /\s+|_+/g;

        // Replace low-upper case to low-space-uppercase
        str = str.replace(/([a-z])([A-Z])/g, '$1 $2');

        // Replace space and underscore with -
        return str.replace(regex, '-').toLowerCase();
        }

    fCC advanced solution: 

        return str.split(/\s|_|(?=[A-Z])/).join('-').toLowerCase()

### D015 #100DaysOfCode - 28.11.18
* Struggled with the next challenge.

### D014 #100DaysOfCode - 27.11.18
* Tried to solve "Apply Functional Programming to Convert Strings to URL Slugs". In my mind I solved it, but fCC told me that the global variable changes. My solution:

        function urlSlug(title) {
        return title.split(/\W/).filter(function(item) {
            return item;
        }).map(function(item) {
            return item.toLowerCase()
        }).join('-')
        }
    
    As far as I know, the .split returns a new array, and the global variable is unchanged when I console.log it. It was a bug. It worked after refreshing the page.
    But there was no need of .map() in this case of course.

    fCC solution: 

        function urlSlug(title) {
        return title.split(/\W/).filter((obj)=>{
            return obj !=='';
        }).join('-').toLowerCase();
        
        }

* Finished Intermediate Algorithm Scripting: "Sum All Numbers in a Range". My solution: 

        function sumAll(arr) {
        let newArr = arr.sort((a, b) => a - b);
        for (let i = newArr[0]+1; i < newArr[1]; i++) {
            newArr.push(i);
        };
        return newArr.reduce((acc, curr) => acc + curr);
        }
        console.log(sumAll([10, 5]));

    As usual, my code is way to advanced. I thought that I needed to return an array and then sum. But we just need the summed total. At least I got to practice .reduce. This is the basic fCC solution: 

        function sumAll(arr) {
            var max = Math.max(arr[0], arr[1]);
            var min = Math.min(arr[0], arr[1]);
            var temp = 0;
            for (var i=min; i <= max; i++){
                temp += i;
            }
        return(temp);
        }

        sumAll([1, 4]);

* Finished Intermediate Algorithm Scripting: "Diff Two Arrays". My solution:

        function diffArray(arr1, arr2) {
        
        return arr2.filter(function(item) {
            return item != arr1.find(function(element) {
            return element === item;
            })
        }).concat(arr1.filter(function(item) {
            return item != arr2.find(function(element) {
            return element === item;
            })
        }));
        }

        console.log(diffArray(["andesite", "grass", "dirt", "dead shrub"], ["andesite", "grass", "dirt", "dead shrub"]));

    I am curious... If I speak out loud, I keep saying to myself: we have to reduce the array to the elements that differ. So can I use reduce?

    Reduce didn't work, but after reading the hints, this is what I came up with: 

        function diffArray(arr1, arr2) {
        
        return arr1.concat(arr2).filter(function(item) {
            return (!arr1.includes(item) || !arr2.includes(item))
        });
        }

        console.log(diffArray(["andesite", "grass", "dirt", "pink wool", "dead shrub"], ["diorite", "andesite", "grass", "dirt", "dead shrub"]));


### D013 #100DaysOfCode - 26.11.18
* Did six functional programming challenges.
* My solution for "Use the reduce Method to Analyze Data: 

        let count = 0;
        var averageRating = watchList.filter(function(item) {
        if (item.Director == 'Christopher Nolan') {
            count += 1;
            return item;
        };
        }).map(function(item) {
        return Number(item.imdbRating)
        }).reduce(function(accumulator, currentValue) {
        return accumulator + currentValue
        }) / count;

    fCC solution was the same except that they used a shorter code and used watchList.filter().length to get numer of elements instead of having a counter:

        var averageRating = watchList.filter(item => item.Director === 'Christopher Nolan').map(item => Number(item.imdbRating)).reduce((accumulator, currentValue) => accumulator + currentValue) / watchList.filter(item => item.Director === 'Christopher Nolan').length;


### D012 #100DaysOfCode - 25.11.18
* Finished "Use the map Method to Extract Data from an Array".
* Finished "Implement map on a prototype".

### D011 #100DaysOfCode - 24.11.18
* Did 4 Functional Programming challenges.

### D010 #100DaysOfCode - 23.11.18
* Did 14 Object Oriented Programming challenges.

### D009 #100DaysOfCode - 22.11.18
* Did 16 Object Oriented Programming challenges.


### D008 #100DaysOfCode - 21.11.18
* Finished "Mutations". I spent way to long on this one. And I am a bit nervous looking at the answer...

        function mutation(arr) {
        let count = true;
        for (let i = 0; i < arr[1].length; i++) {
        if (!arr[0].toLowerCase().match(arr[1][i].toLowerCase())) {
            count = false;
        }
        }
        return count;
        }
        mutation(["hello", "hey"]);
    
    fCC solution (hey! I was not so far of after all):

        function mutation(arr) {
        var test = arr[1].toLowerCase();
        var target = arr[0].toLowerCase();
        for (var i=0;i<test.length;i++) {
            if (target.indexOf(test[i]) < 0)
            return false;
        }
        return true;
        }
    
    fCC intermediate: 

        function mutation(arr) {
        return arr[1].toLowerCase()
            .split('')
            .every(function(letter) {
            return arr[0].toLowerCase()
                .indexOf(letter) != -1;
            });
        }

* Finished "Chunky Monkey". Spent some time on this one. I found a solution, but I wanted to get the exact number at the end of the chunking:

        function chunkArrayInGroups(arr, size) {
        
        let newArr = [];
        let newSize = size;
        
        for (let i = arr.length; i > 0; i = i - size) {
            newArr.push(arr.splice(0,newSize ));
            newSize = i - arr.length;
        }
        return newArr;
        }
    
    fCC solution has a bunch of solutions: https://guide.freecodecamp.org/certifications/javascript-algorithms-and-data-structures/basic-algorithm-scripting/chunky-monkey/

### D007 #100DaysOfCode - 20.11.18
* Worked on "Mutations". Was not able to solve it. Will continue tomorrow. 

### D006 #100DaysOfCode - 18.11.18
* Finished "Slice and Splice" after failing it the other day. I had to make sure that it didnt end up like a nested array. My solution: 

        function frankenSplice(arr1, arr2, n) {
        let newArr = arr2.slice(0);
        for (let i = 0; i < arr1.length; i++) {
            newArr.splice(n++,0,arr1[i])
        }
        return newArr;
        }
        frankenSplice(["claw", "tentacle"], ["head", "shoulders", "knees", "toes"], 2);

    fCC solution: 

        function frankenSplice(arr1, arr2, n) {
        let localArray = arr2.slice();
        for (let i = 0; i < arr1.length; i++) {
            localArray.splice(n, 0, arr1[i]);
            n++;
        }
        return localArray;
        }

* Finished "Falsy Bouncer". My solution:

        function bouncer(arr) {
        let newArr = []
        for (let i = 0; i < arr.length; i++) {
            if (arr[i]) {
            newArr.push(arr[i]);
            } 
        }
        return newArr;
        }
        bouncer([7, "ate", "", false, 9]);
    
    fCC solution: 

        function bouncer(arr) {
        return arr.filter(Boolean);
        }

* Finished "Where do I Belong". I spent a lot of time on this one, only to scrap it all and do this instead: 

        function getIndexToIns(arr, num) {

        arr.push(num);
        return arr.sort(function(a, b){return a - b}).findIndex(function (item, index) {
            return item >= num;
        });
        }

        getIndexToIns([], 1);
    
    fCC solution basic: 

        function getIndexToIns(arr, num) {
        arr.sort(function(a, b) {
            return a - b;
        });

        for (var a = 0; a < arr.length; a++) {
            if (arr[a] >= num)
            return a;
        }

        return arr.length;
        }

    fCC solution advanced: 

        function getIndexToIns(arr, num) {

        return arr.concat(num).sort((a,b) => a-b).indexOf(num);
        }
        getIndexToIns([1,3,4],2);


### D005 #100DaysOfCode - 16.11.18
* Finished "Boo who". To check if a value is boolean I used typeof: 

        function booWho(bool) {
        if (typeof bool === 'boolean') {
        return true;
        }
        return false;
        }
        booWho(null);

    fCC solution (Again more short because I forget how JavaScript works):

        function booWho(bool) {
        return typeof bool === 'boolean';
        }
        booWho(null); 

* Finished "Title Case a Sentence". I used a long time on this one. But I did not cheat. I searched and tried different approaches. First I tried to find a solution with regex, but couldnt get it to work. Then I tried a for loop, but the loop kept breaking when I tried to change values in the array. I think I targeted the values the wrong way. I need to really think about what I am doing, not just try different things blindly. That way I wont learn anything. I see the same thing when trying other approaches. If I dont know how the different methods work, its all a guessing game.

    My solution using for loop: 

        function titleCase(str) {

        let toTitleCase = str.toLowerCase().split(' ');

        for (let i = 0; i < toTitleCase.length; i++) {
        toTitleCase[i] = toTitleCase[i][0].toUpperCase() + toTitleCase[i].slice(1);
        }
        return toTitleCase.join(' ');
        }
        titleCase("I'm a little tea pot");
    
    My second solution using for loop: 

        function titleCase(str) {

        let toTitleCase = str.toLowerCase().split(' ');

        toTitleCase.forEach(function(element, index, array) {    
            array[index] = element[0].toUpperCase() + element.slice(1);
        });

        return toTitleCase.join(' ');
        }
        titleCase("I'm a little tea pot");

    fCC solution (I forgot the "charAt): 

        function titleCase(str) {
        var convertToArray = str.toLowerCase().split(" ");
        var result = convertToArray.map(function(val){
            return val.replace(val.charAt(0), val.charAt(0).toUpperCase());
        });
        return result.join(" ");
        }
        titleCase("I'm a little tea pot");
    
    And fCCs solution using regex, as I thought would be possible, but didnt figure out:

        function titleCase(str) {
        return str.toLowerCase().replace(/(^|\s)\S/g, (L) => L.toUpperCase());
        }

* Tried to finish "Slice and Splice", but could not get it to work. My code: 

        function frankenSplice(arr1, arr2, n) {
        // It's alive. It's alive!
        
        let emptyArr = [];

        let test = emptyArr.splice( n, 0, arr2.slice( 0, n ),arr1.slice( 0, arr1.length ),arr2.slice( arr2.length-n ));

        return test;
        }
        frankenSplice(["claw", "tentacle"], ["head", "shoulders", "knees", "toes"], 2);
    
    The problem was that it returns a nested array. I didnt see that in the fCC console, but saw it in chrome console. So I need to find another way.


### D004 #100DaysOfCode - 15.11.18
* Finished "Finders Keepers". I only finished one challenge today, because I spent some time searching for ways to solve it. I found one way, but then I found a better one right after. I had forgotten that "return" breaks a loop. That was a valuable lesson to relearn. I don't know why, but I really enjoyed it today. Maybe because of my reflections yesterday. 
Here is my first solution: 

        function findElement(arr, func) {
        let num = 0;

        for (let i = 0; i < arr.length; i++) {
            // check that the number is even
            num = arr[i]
            if (func(arr[i]))
            {
                num = arr[i];
                break;
            }
            // if we got here, then i is odd.
            num = undefined;
        }
        return num;
        }
        findElement([1, 3, 5, 8, 9, 10], function(num) { return num % 2 === 0; });
    
    But we dont need break. We can just use return:

        function findElement(arr, func) {
        let num = 0;

        for (let i = 0; i < arr.length; i++) {
            // check that the number is even
            num = arr[i]
            if (func(arr[i]))
            {
                return num = arr[i]; 
            }
            // if we got here, then i is odd.
            num = undefined;
        }
        return num;
        }

        findElement([1, 3, 5, 8, 9, 10], function(num) { return num % 2 === 0; });

    Here it is using .find():

        let num = arr.find(function(element) {
        return func(element);
        }); 
        return num;
        }
        findElement([1, 3, 5, 7, 8, 9, 11], function(num) { return num % 2 === 0; });

    fCC Solution: 

        function findElement(arr, func) {
        let num = 0;
        
        for(var i = 0; i < arr.length; i++) {
            num = arr[i];
            if (func(num)) {
            return num;
            }
        }
        
        return undefined;
        }

### D003 #100DaysOfCode - 14.11.18
* Finished "Repeat a String Repeat a String"- My solution: 

        function repeatStringNumTimes(str, num) {
        const word = str;
        if (num > 0) {
            for (let i = num; i > 1; i--) {
            str += word;
            }
        } else {str = "";};
        console.log(str);
        return str;
        }
        repeatStringNumTimes("abc", 2);

    fCC solution: 

        function repeatStringNumTimes(str, num) {
        var accumulatedStr = '';

        while (num > 0) {
            accumulatedStr += str;
            num--;
        }

        return accumulatedStr;
        }

    I get to stuck to the text in the code window, and struggle to think outside of the box. It seems I automaticly go for a for loop no matter what, and forget to think about all the different methods avaliable. As long as I make it work it's OK I guess, but I think I need to try to approach it differently. It's like I am a bit affraid of the challenges. Affraid that I am not smart enough to solve them and to create neat and effective solutions. I need to trust myself and look at the challenges as a fun way to test myself and to learn. I always learn, and that way I grow smarter. 

* Finished "Truncate a string". My solution: 

        function truncateString(str, num) {
        if (str.length > num) {
            str =  (str.split('').splice(0,num).join('')) + '...';
        } return str;
        }
        truncateString("A-tisket a-tasket A green and yellow basket", 8);

    fCC solution basic (I forgott to check if num is more than three):

        function truncateString(str, num) {
        // Clear out that junk in your trunk
        if (str.length > num && num > 3) {
            return str.slice(0, (num - 3)) + '...';
        } else if (str.length > num && num <= 3) {
            return str.slice(0, num) + '...';
        } else {
            return str;
        }

        } 

    fCC solution advanced: 

        function repeatStringNumTimes(str, num) {
        if(num < 0)
            return "";
        if(num === 1)
            return str;
        else
            return str + repeatStringNumTimes(str, num - 1);
        }

### D002 #100DaysOfCode - 13.11.18
* Finished "Return Largest Number in Arrays". My solution 1 using reduce:

        function largestOfFour(arr) {  
        for (let i = 0; i < arr.length; i++) {
            arr[i] = arr[i].reduce((a, b) => Math.max(a, b));
        }
        return arr;
        }
        largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);

    My solution 2 using for loop and spread:

        function largestOfFour(arr) {
        let newArr = [];
        for (let i = 0; i < arr.length; i++) {
        newArr.push(Math.max(...arr[i]));
        }
        return newArr;
        }
        largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]); 

    My solution using plain old for loops (same as fCC): 

        function largestOfFour(arr) {
        let newArr = [];
        for (let i = 0; i < arr.length; i++) {
            let max = arr[i][0];
            for (let j = 0; j < arr[i].length; j++) {
            if (arr[i][j] > max) {
                max = arr[i][j];
            }
            }
            newArr.push(max);
        }
        return newArr;
        }
        largestOfFour([[17, 23, 25, 12], [25, 7, 34, 48], [4, -10, 18, 21], [-72, -3, -17, -10]]); 

    fCC solution intermediate:

        function largestOfFour(arr) {
        return arr.map(function(group){
            return group.reduce(function(prev, current) {
            return (current > prev) ? current : prev;
            });
        });
        }
    
- Finished "Confirm the ending". My solution: 

        function confirmEnding(str, target) {

        // Change string to array.
        let newArr = str.split(' ')

        // Target last word
        let lastWordInArray = newArr[newArr.length - 1]

        // Counter for use in for loop
        let count = 0;

        for (let i = (lastWordInArray.length - target.length); i < (lastWordInArray.length); i++) {
            if (lastWordInArray[i] === target[count]) {
            count++
            str = true;
            } else {
            str = false;
            };
        };
        return str;
        }
        confirmEnding("Walking on water and developing software from a specification are easy if both are frozen", "specification");

    The much more simple fCC solution: 

        function confirmEnding(str, target) {
        return str.slice(str.length - target.length) === target;
        }
        confirmEnding("He has to give me a new name", "name");


### D001 #100DaysOfCode - 12.11.18
* Arrays and objects. Did 7 - 10 challenges. 
* I learned that array.findIndex(elem) == -1 iterates  through all the elements in an array. If it is false, the code wont continue.
* We use bracket notation ([]) on objects if we want to pass variables (more dynamic) or dot (.) if we know the name.
* Learned to delete key from objects using "delete".
* Learned .hasOwnProperty(key, key, key) and "in" keyword. Can search for several keys separated by commma.
* Iterate through keys in an object with a for...in statement.
* Generate an array og all object keys with objects.keys()
* Finished Basic Data Structure Challenges
* Started Basic Algorithm Scripting
* Finished "Convert Celsius to Fahrenheit"
* Finished "Reverse a string". My solution:

        function reverseString(str) { 
        let newString = '';
        for (let i = str.length-1; i >= 0; i--) {
            newString = newString + str[i];
        }
        return newString;
        }
        reverseString("hello");

    The shorter and more elegant solution (I totally forgot these methods...): 

        function reverseString(str) { 
        return str.split('').reverse().join('');
        }
        reverseString("hello");

* Finished "Factorialize a Number". My solution: 

        function factorialize(num) {
        let count = 1;
        if (num === 0) {
            count = 1;
        } for (let i = 1; i <= num; i++) {
            count = count * i;
            } 
        return count;
        }
        factorialize(5);

    fCC solution: 

        function factorialize(num) {
        if (num === 0) { return 1; }
        return num * factorialize(num-1);
        }
        factorialize(5);

    It's based on recursion, which refers to a function repeating (calling) itself.

* Finished "find the Longest Word in a String". My solution:

        function findLongestWordLength(str) {
        return str.split(' ').sort(function(a, b){
            return a.length - b.length;
            }
            ).pop().length;
        }
        findLongestWordLength("What if we try a super-long word such as otorhinolaryngology");

    fCC solution basic: 

        function findLongestWordLength(str) {
        var words = str.split(' '); 
        // From sting to array

        var maxLength = 0; // Define maxLength
        
        for (var i = 0; i < words.length; i++) { 
            // If i is less than number of elements in array, increase i.
            
            if (words[i].length > maxLength) { 
                // If the words length is bigger than maxLength

            maxLength = words[i].length; 
            // Then set maxLength to match that words length.
            
            }
        }
        return maxLength;
        }

    fcc solution intermediate (I need a refresher on reduce. Watched a YouTube video from The Coding Train): 

        function findLongestWordLength(s) {
        return s.split(' ')
            .reduce(function(x, y) {
            return Math.max(x, y.length)
            }, 0);
        }


### D010 #100DaysOfCode - 15.9.18
* Set up the workout page and got the table to work. I find the current exercises and make inputs to log the number of exercises. 

**Total:**  2.5 h

**Thoughts:** 

### D010 #100DaysOfCode - 13.9.18
* I got the IDs of the sessions to work. But I have to clean up my code. I did a lot of testing. 

**Total:**  1 h

**Thoughts:** 

### D009 #100DaysOfCode - 12.9.18
* I found out that the workout log was copying all the sessions, and not the selected one. Started to try and fix it. It was a bigger fix than I first thought. I need to assign IDs to the sessions as well, so that I am not just comparing names. 

**Total:**  1 h

**Thoughts:** 

### D009 #100DaysOfCode - 11.9.18

**Total:**  1 h

**Thoughts:** 

### D008 #100DaysOfCode - 10.9.18
* Made the edit workout page
* Made the table to contain the workout data
* Managed to connect the edit button of the workout array to the edit workout page with corresponding ID of the selected workout.

**Total:**  1 h

**Thoughts:** 

### D001 - D007 #100DaysOfCode - 1.9.18 - 9.9.18
* Changed how entries are identified: From name to unique ID.
* These IDs are used to fetch the right entries in the arrays.
* Changed the URLs from name to ID (not pretty, but it works)
* "Add workout" now copies the chosen program and session, so that it brings with it the excercises as well. 
* Plus more.

**Total:**  Around 10 h

**Thoughts:** It's a blast getting back to coding again. I don't know why I was so scared and worried. I did nearly give it up. But I pushed myself and I am glad. I really love doing this, and I remember and know more than I think.

### D066 #100DaysOfCode - 17.7.18

* Got the links to change dynamicly based on chosen option in selector.
* Set up the "register workout" button.
* Added a new object to local storage to store the log.
* Added to the log array based on chosen options in the selectors.


**Total:**  2 h

**Thoughts:** I had much better progress today as I managed to finish several tasks. 

I was able to create a new workout object to the log array, and give it name based on chosen options in selectors. But I think it would be better to find the original, and copy it. That way I will get all the exercises as well. I think that will be a nicer and cleaner option. 

I also think I need to add a unique ID, since several of the logged items will have the same name. That way it will be easier to access each item I think.

### D065 #100DaysOfCode - 16.7.18

* Added buttons on the index page for editing programs and sessions.

**Total:**  2 h

**Thoughts:** Slow progress today as well. The sun is frying my brain :D I added buttons for editing programs and sessions on the index page. I got the links to work, but could not get them to change dynamicly. But I know that it's actually pretty easy. 

### D064 #100DaysOfCode - 15.7.18

* Managed to fill the program HTML selector with the registered workout programs.
* Managed to fill the session HTML selector based on the program HTML selector.

**Total:**  2 h

**Thoughts:** My brain worked a bit slow today, but I managed to get the HTML selectors working. It's very cool. I am very happy with what I've learned so far. It seems I've gotten a much better understanding of, at least, the basic JS concepts.

### D063 #100DaysOfCode - 14.7.18

* Started the logging function.
* Added new HTML and JS files.

**Total:**  2 h

**Thoughts:** I started writing the logging functions in the new files I added. It's a challenge, and I had to sit down and write and draw a little. Added the selectors and buttons and started the function that will fill the selectors from the program array.

### D062 #100DaysOfCode - 13.7.18

* Solved the naming problem.
* Added names on the workout programs as titles.

**Total:**  1.5 h

**Thoughts:** Today I managed to print the names of the current session and program as a title. Besides beeing a title, it also works as a breadcrumb, as it shows what program and session the user is currently adding exercises to.

I can almost not believe that I am able to build this. I'm actually a little proud. But now comes the most difficult part: the logging function. I am supposed to add an entry, based on chosen workout, then add reps, notes and date. That is going to be a great challenge!

### D061 #100DaysOfCode - 12.7.18

* Cleaned up code.

**Total:**  1.5 h

**Thoughts:** I refactored code today, to make it more readable and easier to manage. I experienced some trouble with naming variables. The code broke when I used a special name and I could not figure out why. I need to dive deeper into this tomorrow.

### D060 #100DaysOfCode - 11.7.18

* Solved the identify problem (I think).

**Total:**  1.5 h

**Thoughts:** I managed to be able to put level 1 and 2 in the URL when reaching level 3. I then split the URL behond the hash, and used that new array to identify where in the current object the item at level 3 lives. 

Finding the index is not pretty code though, so I have to work on that. But at least it seems to be working. Hooray!!!

### D059 #100DaysOfCode - 10.7.18

* Updated myself on the code.
* Managed to to some tweeks to be able to reuse code.
* Got the delete function on level 2 to work.

**Total:**  5 h

**Thoughts:** Finally back to coding after holiday and a couple of days sick with pneumonia. The couple of days before leaving work was also crazy busy. But now I have a couple of weeks of, and look forward to do a lot of coding. 

I managed to solve level 2 today, and moved on to level 3 of my workout log. That's when it all stopped working.

I have no idea how I am able to identify where I am in the big array of data. I manage to get to level 2 by using the URL. But when moving to level 3, I loose the identifiers. So I have to come up with something smart, so that I don't have to iterate trough the whole array. That would require unnecessary power.

Right now I'm completely lost. I will have to sleep on it.

### D058 #100DaysOfCode - 27.6.18

* Worked on the same challenge as yesterday. 
* Managed to use .findIndex() to get the program matching the name after the hash in the subpage URL.

**Total:**  1 h

**Thoughts:** I still didn't manage to solve the problem with reusable code. So I changed focus to get something other to think about. I managed to use findIndex() to find the program that the subpage referes to. I will continue building block by block tomorrow.

### D057 #100DaysOfCode - 26.6.18

* Tried to make better code by reusing the code that produces the DOM elements.

**Total:**  1 h

**Thoughts:** It's been a busy day, so I only got to code for one hour. 

I'm trying to reuse some code, without luck. I don't want to use the same code on every subpage, so I figured that I could pass an argument to be able to generate DOM elements based on the value passed in. But there seems to be some conflict with the delete function. 

I'll look more into it tomorrow. 

### D056 #100DaysOfCode - 25.6.18

* Explored more object methods.
* Edited the URL of the subpage to remove spaces and keep the letters lower case. 
* Managed to add sessions to the program that the subpage is pointing to. 

**Total:**  2 h

**Thoughts:** Went slow today. A bit tired after much work and very hot weather. But I still made progress. I seem to have everything in place to build the rest of the functionality of the subpage. I just need to set ut the button and create new DOM elements. 

What I learned today: 

* You can not store functions / code in local storage

### D055 #100DaysOfCode - 24.6.18

* Tried different methods and variants of objects to see what could work best, and to make the code better.
* Added new JS files to keep functions separated from other code.
* Added HTML to subpage.
* Managed to retrieve from and add to the program that the subpage pointed to. 

**Total:**  2 h

**Thoughts:** More progress. This is going much faster that anticipated. I did spent a great amount of time today to test out different things with objects. But that was to learn and test different approaches. 

### D054 #100DaysOfCode - 23.6.18

* Set up third party library UUID, to get random ID-number for each program. Used as a unique identifier.
* Set up the delete button
* Set up a message if no programs are defined
* Added an edit button that takes you to the edit page with the unique ID of the pressed program behind a hash in the URL.
* Had so much use of the dictionary I made.

**Total:**  2 h

**Thoughts:** Made a lot of improvements today. Connected the programs to unique subpages that retrives info from that specific program. SO MUCH FUN!

### D053 #100DaysOfCode - 22.6.18

* Set up the DOM structure for each program
* Built a function to generate DOM elements
* Built a delete button with a function inside of the generateToDOM function!
* Built a function that rendered the programs based on the generateToDOM function

**Total:**  2 h

**Thoughts:** This is going much better that I first thought. But I'm not going to get to cocky. I know the problems will come soon enough. I have some advanced functions coming up. 

But I ABSOLUTELY LOVE IT! I want to code all day. I think about it all day and can't wait to get out of bed to code, or to start coding in the evenings. 

What I learned today:

* I could not get the .generatoeToDOM function to actually generate any elements. The was an error in the render function, and the console gave me this error message: "Argument 1 of Node.appendChild is not an object".

    That meant that I was missing an argument. So I had to look at the .generateToDOM function. And sure enough. I had forgot to return anything! :flushed:

* All my programs in the array got multiplied when I added a new program. I had to clear the innerHTML of the DIV before render them again.

### D052 #100DaysOfCode - 21.6.18

* Created a main object
* Created an event listener that made a copy of the main object with a new name
* Added local storage
* Fetch from and save to local storage

**Total:**  3 h

**Thoughts:** What I learned today:

* I could not for the life of me get document.querySelector to work. That was because I had the script tag in the header. So it run before the DOM and could therefore not find the element. 
* I could STILL not get document.querySelector to work:

      document.querySelector('#add-program-button').addEventListener('click', (e) => {
         e.preventDefault()

    The page still refreshed, and it would not log to console. The problem was that I shortened 'event' with 'e'. That is of course not possible.

* I went on to save the objects to local storage. But it kept getting overwritten on refresh, and / or overwritten when adding a new program. The cause was the placement of the code. So after moving it around a bit, it worked like a charm. This is part of CRUD, so it has to happen in the right order. First I create an array from items in local storage, then I add to that array when pressing the button, then saving the new array to local storage.

        // Create program array from localStorage
        const createPrograms = () => {
            const programJSON = localStorage.getItem('program')

            try {
                return programJSON ? JSON.parse(programJSON) : []
            } catch (e) {
                []
            }
        }

        // The array to hold the different programs
        let programs = createPrograms()

        // Save programs to local storage
        const saveProgram = (program) => {
            localStorage.setItem('program', JSON.stringify(program))
        }

        // Listen for button press.
        document.querySelector('#add-program-button').addEventListener('click', (event) => {
            
            event.preventDefault()

            const input = document.querySelector('#add-program')
            const inputData = ''

            // Check to see if input has value
            if (input.value) {
                programs.push(new program(input.value))
                input.value = ''
                saveProgram(programs)
            } else {
                alert('You need to enter a workout name')
            } 
            
        })

### D051 #100DaysOfCode - 20.6.18

* Started coding project
* Created all the different HTML pages, and added simple basic HTML code
* Started coding the main object and tested out different functions

**Total:**  2 h 

**Thoughts:** Finally! I am back to coding. But to be honest, I was a bit terrified of starting. Fear of failure and self doubt washed over me like a tsunami. Even though I new this would happen, I wanted to procrastinate so badly.

It's so much harder to code on your own, instead of following along a course. But I really need this now, so that I don't forget. But I do feel that I have a lot more controll this time around. I feel that I have a better knowledge of all the methods and code, and judging by my notes, I'm not totally lost. 

This will be exciting!

[Image of mockups](https://ibb.co/f291vJ)

I think I have a good plan of how this should work out. It's probably totally wrong. But we'll see.

### D050 #100DaysOfCode - 19.6.18

* Further planned my project

**Total:**  2 h 

**Thoughts:** Today I printet my mockups, wrote notes on them and tried to get an overview of the code needed, and the structure. 

[Image of mockups](https://ibb.co/f291vJ)

I think I have a good plan of how this should work out. It's probably totally wrong. But we'll see.

### D049 #100DaysOfCode - 18.6.18

* I sketched my project on paper, wrote down the features and the flow of the program
* Made mockups of project in Indesign
* Think I figured out how it should work in code

**Total:**  2 h 

**Thoughts:** I sat down to really plan my next project. It's easy to get carried away, so in the end I put a lot of features on the nice to have list. I'm afraid I would make it to hard for myself, and that it would be easier to give up. But I also wanted it to be a real challenge. And it will be. It includes local storage, several subpages, external library, objects, copies of objects and more. 

I have to admit that I am a bit afraid to start. But I wont give myself a time limit. I will work on it till I finish it. Hope to start actually coding tomorrow. But it's nice to let the idea float around in my head for a couple of days.

### D048 #100DaysOfCode - 17.6.18

* Did a couple of challenges
* Finished the course (sort of)
* Started to plan my next project on pen and paper

**Total:**  3.5 h 

**Thoughts:** I finished the course, except the later added section about Babel and Webpack. I figured that I need to do a lot more basic coding before I spend time on advenced stuff.

So my next step will be to start building a project, and continue my fCC curriculum.

I was so excited to start programming again, so I spent some time this afternoon and evening to plan my project which will be a workout log. I plan to use local storage and hopefully convert it to Node later. 

Right now I am just writing down all the features I would like. Whether I  will be able to make it remains to be seen.

### D047 #100DaysOfCode - 16.6.18

* Solved the challenge
* CSS

**Total:**  1.5 h 

**Thoughts:** I solved the challenge two different ways. And yet, none were like the instructors. I guess "best practice" comes with experience and no matter what, will be somewhat subjective since there are so many ways to solve a problem.

### D046 #100DaysOfCode - 15.6.18

* Added some styling
* Started a string to array and then to string challenge

**Total:**  1 h 

**Thoughts:** Today was the first time I sped up the video to two times the speed. I know CSS good enough. But suddenly the instructor added some JS challenges in there, so I had to hit the breaks and try to solve it. I will continue tomorrow. To tired today. 

### D045 #100DaysOfCode - 14.6.18

* Did a bunch of challenges about asynchronous and async / wait.
* Finished section

**Total:**  2.5 h 

**Thoughts:** I am getting close to ending this course. It's only a couple of sections left. The next one is about styling, so I'll blast through that.

I've learned a lot by taking this course. But I also think I've figured out how I could learn even more. I think by making small programs on each topic / section you learn, you will remember it much better. 

By just following along the course, even though you feel you get the grasp of it, I think that when moving on, you remember less and less. I've reduced this by writing my own dictionary. That helps me remember much better. But it would be even better to build a couple of programs as well.

I felt that I really understood objects and arrays much better this time, but I am afraid the knowledge dissapears when I don't use it.

I will remember this when taking new courses and try to implement it.

### D044 #100DaysOfCode - 13.6.18

_Morning (04.30 - 06.00)_

* More promises and challenges

_Afternoon (15.00 - 16.00)_

* More promises and another challenge

**Total:**  1.5 h 

**Thoughts:** I did a lot of challenges today. Not to hard, and I can see how to do it by looking at prevoius examples done with the instructor. But by repeating it so many times, it's easier to really get a grip on how it works. Guess I'm ready for fetching some API data as well now!

### D043 #100DaysOfCode - 12.6.18

_Morning (04.30 - 06.00)_

* Explored promises
* Did promises challenges

**Total:**  1.5 h 

**Thoughts:** Finally got to code for one and a half hour today. I did some promises challenges and switched out some callback functions for promises. Pretty cool.  

### D042 #100DaysOfCode - 11.6.18

_Evening (21.30 - 22.00)_

* Did a closure challenge.
* Started promises.

**Total:**  0.5 h 

**Thoughts:** I only got half an hour of coding today. The reason is that I want to go to bed early, and get more coding done in the morning again. I've gone to bed to late the last couple of days, soI have to get back on track. 

### D041 #100DaysOfCode - 10.6.18

_Morning (07.00 - 08.30)_

* Further explored asynchronous and synchronous Javascript
* Learned about closures and currying
* Started a new challenge

**Total:**  1.5 h 

**Thoughts:** Closures was a bit tricky at first, but I managed to wrap my head around it after a while. It helps to write it down in your own words. So my dictionary is helpful in more than one way. 

I also added my first entries i my commonplace book today. That was fun. I think I finally have found a good way to learn / remember better from books.

### D040 #100DaysOfCode - 09.6.18

_Evening (20.30.00 - 22.00)_

* Explored asynchronous and synchronous execution. 

**Total:**  1.5 h 

**Thoughts:** I think I have been through this at one time earlier. But it was useful to revisit it. It was explained in a very good way. This instructor is really good. 

### D039 #100DaysOfCode - 08.6.18

_Evening (23.00 - 00.00)_

* Solved a HTTP request challenge

**Total:**  1 h 

**Thoughts:** It's not easy to get time to code when summer has come. A neighbour came over to talk over a couple of beers. It was nice though. I do need a bigger network and some more friends. But I didn't get to register to my GitHub before 00.00, so I got another gap :( 

### D038 #100DaysOfCode - 07.6.18

_Evening (21.00 - 22.00)_

* Solved the bug
* Started a HTTP Request challenge

**Total:**  1 h 

**Thoughts:** I got the code working. As usual it was a typo. Started a new challenge that I think I will be spending some time on. I love this GET methods and API stuff, and I'm itching to use it in some projects.

### D037 #100DaysOfCode - 06.6.18

_Evening (20.45 - 22.45)_

* Started to explore HTTP Request.

**Total:**  1 h 

**Thoughts:** Only got to code for one hour today. The HTTP request is a bit technical, but fun. Only I did not get the code to work, even though I did exactly like the instructor did. I have to explore further tomorrow.

### D036 #100DaysOfCode - 05.6.18

_Evening (20.30 - 22.00)_

* Did a challenge based on setters and getters. 

**Total:**  1.5 h 

**Thoughts:** Spent a long time on a challenge that was super easy. I just thought it was supposed to be hard. I went straight into the trap lol. A proof that "think like a programmer" has some truth in it. If nothing else it just means trust yourself and as long as it works, it works. 


### D035 #100DaysOfCode - 04.6.18

_Afternoon (15.00 - 16.00)_

* Did a challenge
* Learned about classes and subclasses.

_Evening (21.15 - 22.15)_

* Learned about getters and setters.

**Total:**  2 h 

**Thoughts:** Today I learned about classes and getters and setters. Three new concepts I hadn't heard about. Very interesting stuff. 

Other that that my back has started hurting pretty badly again. I wonder if it's the workouts. I'm only doing bodyweight training, so I didn't think the Scheuermann's disease would bother be. Maybe I've been working out too much. I'm nervous about it. Going to the doctor in the morning. 

### D034 #100DaysOfCode - 03.6.18

_Evening (21.30 - 23.00)_

* Did a challenge 

**Total:**  1.5 h 

**Thoughts:** Did another challenge on the hangman challenge. Spent a lot of time failing due to a typo again...

Then solved the rest of the challenge pretty easily. It was fun.

### D033 #100DaysOfCode - 02.6.18

_Morning (07.00 - 09.00)_

* Did a challenge 

**Total:**  2 h 

**Thoughts:** I solved the challenge from yesterday. Again, in another way than the instructor, lol. And again he's is of course more effective and short. But it's good learning non the less. 

I thought about the challenge when I went to bed yesterday, when I woke up today at 05.00 (I went back to sleep), so I had kinda solved it even before I sat down this morning. 

Very happy for my friend who got a job. 

### D032 #100DaysOfCode - 01.6.18

_Morning (04.30 - 06.00)_

* Did a challenge

_Evening (22.00 - 23.00)_

* Worked on an object challenge 

**Total:**  2.5 h 

**Thoughts:** What a day. The neighbouring house burned to the groud, it's the first day of summer and I finished a great book. Also did a challenge before work, but did not manage to solve the last one before bed. 

Very happy for my friend who got a job. 

### D31 #100DaysOfCode - 31.5.18

_Afternoon (15.00 - 16.00)_

* Prototype chain for arrays, strings, numbers and objects.

_Evening (21.30 - 22.30)_

* Solved a prototype / method challenge.

**Total:**  2 h 

**Thoughts:** I was surprised I managed to solve todays challenge so fast. That was nice. I remembered that even though we pass more arguments than the functions parameters, they are still avaliable. So I looked that up in my dictionary. It's such a great feeling to find the answer in something I myself have written. I seem to remember it better. I haven't watched the solution yet. I guess it's still more elegant, but I managed to solve it, so I'm happy. 

### D30 #100DaysOfCode - 30.5.18

_Evening (21.00 - 22.00)_

* Primitives and objects / object prototypes.

**Total:**  1.5 h 

**Thoughts:** Really useful to be reminded how objects and object prototypes, and the prototype chaining works. 

It's so hot that I'm actually coding on the porch. That is really lovely.   

### D29 #100DaysOfCode - 29.5.18

_Morning (04.45 - 06.15)_

* Solved the challenge
* Further exploring prototypal inheritance

**Total:**  1.5 h 

**Thoughts:** I solved the challenge. It was of course less complicated than my fist tries. It was more or less the same solution as the instructor, except he had a slightly more elegant way to do it, which I should have thought of.

When reading the comments this time, I got a little down. People had posted all kinds of smart and more advanced solutions.

But I'm sure they have more experience or have used way more time on it to find alternative solutions. Time that I don't have. So I have to be patient and continue on, knowing that knowledge comes with experience. That's just the way it is.  

### D28 #100DaysOfCode - 28.5.18

_Morning (04.45 - 06.15)_

* Started new section.
* Exploring function constructors and object oriented programming.

_Evening (20.15 - 21.45)_

* Got stuck in a challenge

**Total:**  3 h 

**Thoughts:** The object oriented programming is smart and fun. It makes for some really functional programming. I used it in the book list, but it's nice to recap. 

I also think I came up with my next project. It will be a work out list. I can implement a lot of what I have been learning now; time, local storage, several pages and more. It will be a big and andvanced project, but fun I think. I need to sketch it out. 

Tried to take on a challenge by the end of the day. My head worked a bit slow, and I could find out how to solve it. I will continue tomorrow.

### D27 #100DaysOfCode - 27.5.18

_Morning (06.00 - 09.00)_

* Type coercion.
* Catching and throwing errors.
* Handling application errors.
* Couple of challenges.
* Ended section

**Total:**  3 h 

**Thoughts:** I can't wait to implement all of these things in my next projects. It's going to clean up my code so much. I'm really excited about it. 

### D26 #100DaysOfCode - 26.5.18

_Morning (06.00 - 08.00)_

* Arrow functions and ternary operator
* Truthy / falsy 

**Total:**  2 h 

**Thoughts:** I got great explanations and good examples of both arrow functions and ternary operator. I haven't used those earlier. I will definitely use them from now on, now that I have a good understanding of what they replace and their pros and cons.

### D25 #100DaysOfCode - 25.5.18

_Morning (04.45 - 06.15)_

* Finished dates.
* Did a sorting challenge.
* Started arrow functions. 

**Total:**  2 h 

**Thoughts:** I'm far into the course now, moving into more advances stuff, starting with arrow functions. I'm looking forward to be done with the course, so I can start building stuff again. Doing both will make the course take so long, since I have limited time during a day. But going through all this again, and building my own dictionary will be a great fundament for going forward. 

### D24 #100DaysOfCode - 24.5.18

_Morning (04.30 - 06.30)_

* Learned about time and dates in JavaScript.
* Added Moments library and explored it's methods.
* Did two challenges.

**Total:**  2 h 

**Thoughts:** Another library added. This time to make it easier to work with time. Doing it in vanilla JS generates a lot of code and takes a long time. Moments was really easy. I start to see the benefits of third party libraries now.  


### D23 #100DaysOfCode - 23.5.18

_Morning (04.30 - 06.00)_

* Did a challenge
* Learned about live updating windows with .window and the "storage" event.

Evening (21.45 - 22.30)_

* Started exploring dates. Thats also new to me.

**Total:**  2.15 h 

**Thoughts:** It's amazing what you can do with programming. Switching out local storage with server will be fun. 

The code for this note app is getting complicated, and the instructor is moving a bit fast. I have to take it slow and review all the new code, not just type it in.  

Started to learn about "new Date()". There are so many methods!

### D22 #100DaysOfCode - 22.5.18

_Evening (21.30 - 22.00)_

* Continued exploring updating local storage.

**Total:**  30 min 

**Thoughts:** I only got 30 minutes of coding done today. I got a bit late yesterday, so I had to get some sleep. I also had stuff to do this evening. I'll catch up tomorrow. 

### D21 #100DaysOfCode - 21.5.18

_Morning (06.45 - 09.00):_

* Got stuck in a challenge by stupid mistake. 
* Solved a couple of additional challenges
* Started exploring .location and dynamic linking between pages.

_Evening (21.30 - 22.15)_

* Learned how to use location and some of its properties and methods. 
* Did a .location challenge.

**Total:**  3 h 

**Thoughts:** I spent almost an hour debugging, only to find out that I was somehow missing a whole function. I must have deleted it by accident. I didn't realize, and used a different function, so I didn't get any errors, only the wrong result. _Facepalm_ when I realized it.

My head was not with me today. I worked slow and had trouble concentrating. The code gets more complex, and the doubts begin to creep in again. Again: I have to use this in my own projects to really learn it. 

But I started exploring .location and dynamic linking. That is exciting and useful.

### D20 #100DaysOfCode - 20.5.18

_Morning (06.30 - 09.00):_

* Learned about debugging. I hadn't used that before, so that was very useful.
* Solved a DOM challenge.
* Added third party library called uuid. It creates a random ID.

_Evening (22.30 - 23.00):_
* Went through how to use the third party ID to delete items. 
* Started a new challenge.

**Total:**  3 h 

**Thoughts:** I was a little surprised to include a third party library at this point, especially to create an ID for each todo in the todo app to be able to delete the todo. 

I thought that could be done with ".this" or some sort of counting. That when you clicked the delete button, it was easy to make code that said "delete the note that I currently clicked". 

The instructor said that it was perfectly fine to use third party libraries, and that it's something everyone does. Why invent the wheel and use a lot of time to create something that has already been made. It's better to focus on making your program good, no matter which way.

That sounds reasonable. But for learning, I will try to keep third party libraries to a minimum, if it doesn't need something super complex. The Node course included a lot of it, so I will learn to implement libraries soon enough. First I want to have JavaScript in my fingers. 

### D19 #100DaysOfCode - 19.5.18

_Morning (05.30 - 07.00):_

* Did a bunch of refactoring and completed a refactoring challenge 

**Total:**  1.5 h 

**Thoughts:** I only got to code this morning. We had a wonderful day in Oslo together. I was t tired to to any more coding when I got home. 

### D18 #100DaysOfCode - 18.5.18

_Morning (06.30 - 09.00):_

* Continued the form challenge and solved it. I of course had an inferior solution compared to the instructor, but it was not far off. It was at least the same logic. 
* Spent a lot of time studying the instructors code and code from other attendees in the Q&A. 

_Evening (21.30 - 23.00)_

*  Finished the form section, and moved on to local storage.
* Learned about CRUD: create, read, update and delete. The four operations that makes up the data storage mechanism.   
* Did a JSON challenge.

**Total:**  4 h 

**Thoughts:** Today I finished forms and went on to JSON and local storage. Again I am so glad I chose to take this course before continuing the one for Node. Now I am going through a lot of the stuff that the Node course took for granted that you knew. Going through it and putting everything in my dictionary will make me much more robust for the Node course. 

I also spent some time today examining the consept of commonplace books. That seems very exciting and it's just what I am looking for to be able to better learn from what I read. The question is do I make it digitaly or with handwriting. Need to think about that. I have come to realize that without processing what I read some way or another, it's just reading words, and learning less.  

### D17 #100DaysOfCode - 17.5.18

_Morning (05.30 - 07.00):_

* Did a form challenge
* Added entries to my dictionary  

_Evening (22.00 - 22.30)_
* Checkboxes 
* A new challenge. Got to tired to finish.

**Total:**  2 h 

**Thoughts:** Been out all day celebrating Norways national day. So not much coding today either. But it's more than nothing after all. Working with fomrs and trying to implement checkbox functionality in a challenge. Got to tired to solve the last part. 

### D16 #100DaysOfCode - 16.5.18

_Morning (05.30 - 06.15):_
 
* My laptop would not boot, so I spent all my time trying to fix it, with no luck so far.
 
Midday (2 h):_

* Spent lunch and some other free time to fix the laptop. Now it's working again. 

**Total:** 2.45 h (making my laptop work)

**Thoughts:** Almost been no coding today and that bothers me. And at the same time I'm kinda glad it does. That means that it has become a habit and that I am really enjoying it. 

### D15 #100DaysOfCode - 15.5.18

_Morning (04.30 - 06.15):_
 
* Got stuck with a challenge
* Fixed it after realizing I used the wrong method. 
 
_Evening (21.30 - 22.00):_

* Continued course, working with forms.
 

**Total:** 2.15 h

**Thoughts:** I was stuck today, and had to take a peak at the solution. Of course I should have figured it out. I used .forEach() instead of .filter(), which is a big difference. The first one returns a boolean, the second a new array. So no wonder nothing worked.

### D14 #100DaysOfCode - 14.5.18

_Morning (05.30 - 06.00):_
 
* Continued DOM manipulation.
* Added entries to dictionary. 
* Did a couple of challenges. 

_Midday_ (20 min)
* Got to use .appendChild(), .forEach() and .remove(), which I just leared on a project today. Just a smal thing, but cool non the less. 

_Evening (21.00 - 22.00):_
* Continued course with advanced filtering and DOM manipulation. Some tougher challenges. 

**Total:** 2.50 h

**Thoughts:** The challenges and lectures are really getting advanced. And I start to notice how fast and easy I forget, when things move faster. That's why it's so important to actually use it in projects. 

I got to write a smal code at work today. It included what I just learned, so that was fortunate. It was fun to be able to solve it. I would not have been able to just a couple of months ago. 

### D13 #100DaysOfCode - 13.5.18

_Morning (06.30 - 09.00):_
 
* learned about .reduce() by reading the comments for the current tutorial section. I haven't used that earlier. Nice method.
* Learned how to set up a local web server. Super useful!
* Added entries to dictionary

_Evening (20.30 - 22):_
* Learned the methods .createElement() and .appendChild(). I haven't used those yet.
* Created DOM elements based on filtering and looping through arrays in the javascript file.
* Decided to buy a new laptop and create a new setup at home, with home server and a docking station for the laptop. 

**Total:** 4 h

**Thoughts:** Today I learned about and used a couple of new methods. That was great fun and super useful. I think I would be able to simplify my book list projects with some of these methods. I will look into it later. Right now I am set to finish this course before doing anything else. 

I can't believe that is almost a year since I decided to start this project. Last summer, while cutting the grass, I listened to an episode of Code Newbie. They interviewed Kallaway about his 100 Days of Code. I was spellbound. I listened to that episode several times, taking notes of everything they said.

While cutting the grass again yesterday, I thought about how far I have come. It's only a smal step, but it's a step none the less, and in the right direction. There is no other way than to keep working, every day. And maybe in another year from now, I will have taken a _couple_ of steps. Who knows.   

### D12 #100DaysOfCode - 12.5.18

_Morning (05.30 - 09.00):_
 
* Continued the course and completed new challenges.
* Added entries to the dictionary.
Solved advanced challenges using my own dictionary! Cool!

_Evening (21.00 - 23.00):_


**Total:** 5.5 h

**Thoughts:** I am so glad I decided to take this basic course again. There is just so many things I understand better now. At this point in the course, the challenges are getting more advanced. Today I got stuck on a array problem. But I managed to solve it by reading my own dictionary. How cool is that! That means that I probably understood it when I wrote the entry, and that I managed to explain it good enough so that I understood it when reading it. But got to keep the ego down and code on.

I finished the second part of the challenge pretty easy. I spent time trying to figure out if I could refactor it. I did come up with another solution, but it actually required more code than the original.  

### D11 #100DaysOfCode - 11.5.18

_Morning (06.00 - 07.00):_
 
* Continued the course and completed new challenges.
* Added entries to my dictionary.
* Got stuck in the last array challenge.

_Evening (21.30 - 22.30):_
* Solved the challenge I was stuck on. I came up with a different solution than the instructor though.

**Total:** 2 h

**Thoughts:** Got stuck with a challenge today. That was kind of nice. Thought about it all day at work. I solved it when I came home, albeit using another solution than the instructor. But that's OK. Now I learned two ways to do it! I haven't worked much with arrays, so this is very usefull. 

### D10 #100DaysOfCode - 10.5.18

_Morning (05.30 - 08.00):_
 
* Continued the course and completed new challenges.
* Added entries to my dictionary.

_Evening (19.30 - 22.00):_
* Venturing into arrays. Learned a couple of new things I didn't know. 
* Did a lot of challenges. Pretty easy.

**Total:** 5 h

**Thoughts:** Got in 5 hours today. I'm happy with that. I am really hooked to this programming now. I think it has become a natural part of my life. I wake up, make coffee and code. When the kids are in bed, I sit down to code. And I look forward to doing it every time. That is super fun. 

### D9 #100DaysOfCode - 9.5.18

_Morning (05.30 - 06.30):_
 
* Continued the course and did a couple of challenges.
* Added entries to my dictionary.

_Afternoon (18.00 - 19.00):_
* Got in an hour of coding when the others were in town. Did a challenge related to objects and methods. 

_Evening (21.30 - 22.45)_
* Finished a couple of videos with a bunch of challenges. The course is good, and I really learn much more the second time around. 

**Total:** 3.15 h

**Thoughts:** Got up a little late today due to bad sleep. And it's really warm. 

I am enjoying the tutorial. The instructor is good, and he explains things in a different and simpler way. Besides, since I've been through this one time already, I have a much bigger understanding of what he is talking about. And I pick up on things that I either had forgotten, misunderstood, or not learned properly. So thats fun. But again, it's also a bit easy, so the feeling of "doing great" and progressing is a false one. 

### D8 #100DaysOfCode - 8.5.18

_Morning (05.30 - 06.30):_

* Continued to review the basics of JS.
* Updated my dictionary with a lot of entries.

_Miday (2 h):_
* Startet coding a calculator to be used on a clients website!

_Evening (21.30 - 22.45):_

* Finished the calculator and sent it to my colleague whos in charge of the customer. I only finished the functions. It's without styling for now.

**Total:** 4.15 h

**Thoughts:**
My colleagues at work have figured out that I'm learning to code. I've tried to be quiet and modest about it, but now the requests is starting to come... Today one of my colleagues asked me if I could look at a simple calculator for a client. The purpose was (simply put) to calculate a trucks total loading weight based on the weight of the truck, the trailer and more. 

I noticed that when the question came, I immediately went into some sort of defensive stanse. I became tense, sceptical and got a snappy tone in my voice. Luckily I realized it pretty fast, and tried to get out of it. It was just so scary to get that question. But at the same time it was very exciting. This is after all what I am working towards, and I started right away. I managed to finish it by this evening. It needs some cleaning up and refactoring, but it works and can be testet while I pretty it up. 

So now I actually appreciate the request. It was / is a nice challenge and good practice for me.    

### D7 #100DaysOfCode - 7.5.18

_Morning (05.15 - 06.15):_

* Continued to review the basics of JS.
* Updated my dictionary with a lot of entries.

_Evening (21.00 - 22.00):_

* Did a couple of challenges.
* Added to the dictionary.

**Total:** 2 h

**Thoughts:**
I learned something new today, and that was setting default parameter values when defining a function. I can't remember learning that earlier. Very useful.

Really loving programming and JavaScript, even though I'm back at basics. My plan is to finish the course, then start a new project. 

Also read some really interesting pages of "Ego is the enemy" today, which I really related to and is important to remember going forward. 

### D6 #100DaysOfCode - 6.5.18

_Morning (07.00 - 09.00):_

* Continued to review the basics of JS.
* Updated my dictionary with a lot of entries.

_Evening (21.00 - 22.30):_

* Continued the course.
* Added yet more to the dictionary.

**Total:** 3.5 h

**Thoughts:**
Even though I know every concept I am currently going through in the Udemy course, I learn so much more by doing it again, and using it as a base for my dictionary. I learn new ways of explaining things, and new definitions and new words. This is actually just what I felt was missing. So I'm kinda happy about the choice after all. Even though, again, going through this tutorial makes me feel like I am learning a lot. The truth is that using it in my own projects are what works best. But I'll blast through it, then start building again.

### D5 #100DaysOfCode - 5.5.18

_Morning (06.00 - 08.30):_

* Started viewing basic JS-videos again to refresh terminology, concepts, methods and so on.
* Created a dictionary in which I explain to myself the different coding terminology. Hopefully it will help me remember better. It's also fun to do.
* Went through my fCC account to prepare a restart of the algorithm scripting.

_Evening (21.30 - 23.00):_

* Continued watching videos, doing challenges and adding to my code dictionary. 

**Total:** 3 h

**Thoughts:**
As I go through some of the basics again, I realise how much I have forgotten, but at the same time how much I know and have learned since I started last September. That gave me a boost. And while flipping through Haverbekes book, I noticed that I now understand a lot more than I did last time, which then led me to quit reading it. 

I've also (totally subconsciously), began to solve a couple of the advanced challenges on fCC. I seem to have a couple of ideas on how to solve them. At least in my head :D

All in all a good day.

### D4 #100DaysOfCode - 4.5.18

*Morning (05.45 - 06.15):*

* Slept bad tonight, so I had to get up a little later than usual. When I sat down to code, Visual Code had updated, and the Git link was broken. I couldn't get it working again. So not much productive work this morning. 

_Evening (21.00 - 23.00)_
* Managed to get Git working again.
* Updated my GitHub with more repositories.
* Added explanations to my dictionary. 

_Resources:_
* Udemy.
* Podcast: "Learn to Code With Me" S4E17.

**Total:** 2.30 h

**Thoughts:**
 I've been thinking all day. I've decided to drop the Node.js course for now to focus on pure JavaScript. I'm going to restart fCC, watch my JS videos on Udemy again to refresh my knowledge. I also need to review my goals. 


### D3 #100DaysOfCode - 3.5.18

*Morning (05.30 - 06.00):*

* Trying to solve a bigger challenge given by the instructor. It involves setting a default address in the weather app by writing to file. I had to review earlier tutorials to get going. It's tough, but as I wrote yesterday; this is what I learn the most from, not just following along. I managed to set up writing to file, now I need some code to be able to overwrite it, and call it if no new address is set.

*Evening (20.30 - 22.00):*

* I managed to solve the challenge. It's not pretty, but it works. There is now a default address set if the user don 't input one, and there is a possibility to overwrite the standard if one wishes.
* Moved onto section 5 about web servers and application deployment. 
* Made my first local server!

**Total:** 3 h

**Thoughts:** I did not get much joy out of solving the challenge. I know that the code was bad, and I got a bit frustrated. As I commented yesterday; many of the things I go through in the tutorials are soon forgotten. I concentrate more on following the instructor. I need to change that. 

It's exciting to move on to the server stuff, but I realize that need a better JS fundamen before moving forward with Node. I need to think! 


### D2 #100DaysOfCode - 2.5.18

*Morning (05.30 - 06.15):*

* Learned about ES6 Promises. A function that works as an alternative to callback functions. It's a function that takes one argument, and it is either resolved or rejected and it can't be called more than once, as opposed to regular callbacks. It's advanced when not having enough experience.

*Midday (2 h):*

* Continued with advanced ES6 Promises, and Promise chaining and "catch". Catch is an alternative to the error handler which is preferable to use when chaining to avoid errors.
* Managed to solve a challenge related to chaining Promises. Even though it was a success, it's important for me to realize that it was mostly studying what I went through earlier in the tutorial. But I did not just copy/paste, so I feel that I grasped the Promise method a _little_ better after this challenge.
* Did a quiz related to Promises. Got 5 out of 6 correct.
* Installed Axios, a library simplifying Promises so that we don't need to wrap "resolve" and "reject" in "new Promise" functions.
* Chaining Promises with Axios makes the code easier to handle, since we don't get as much indentations. But it is pretty complex. I fell off pretty quick in this tutorial, but managed to get a better understanding afterwards by reading through the code and commenting and not just keeping up with the instructor.

_Evening (21.00 - 22.00)_

* Trying to solve one of the instructors last challenges for the curent section. It's super though. Will continue tomorrow.

**Total:** 3.45 h

**Thoughts:** The last tutorials were complicated, but I managed to get through. But trying to solve the last big challenge from the instructor tells me that tutorials is a bit dangerous. They make you feel like you learn a lot, in a sneaky kind of way. It's not that I don't learn anything, but when trying to do stuff on my own, I realize how little I know. So I will try to add these new features, and only then continue with the course. That will teach me the most. 

### D1 #100DaysOfCode - 1.5.18

*Morning (04.45 - 06.36):*

* Continued my Node course on Udemy, building a weather app. It's fetching the address and live weather data via APIs. That is super cool, and it's so exciting to finally get some experience connecting things together through HTTP requests and APIs. 
* Installed [JSON view](https://github.com/gildas-lormeau/JSONView-for-Chrome) Chrome extension. Super useful when working with JSON data.
* I've used a lot of callback functions in this project. I need to get a better understanding of how that works in relation to the call stack, event loop etc.  
* I managed to solve all the challenges the instructor gave. 

*Afternoon (17.00 - 18.00):*

* Updated the theme on my WP-blog.
* Updated my GitHub Log and README for round 3.
* Learned the GitHub markdown and GitHub emoji markup.
* Installed a plugin for VS Code that let me preview GitHub markdown. Super useful. 

*Evening (21.00 - 22.00):*

* Chained callbacks together in the Node.js weather app, creating a dynamic app that fetches weather based on the address the user types in.

**Total:** 3.50 h

**Thoughts:** Node.js is very cool. And chaining different callbacks together is powerful. But it gets complicated fast. I stopped several times, needing to get an overview and understand how it was all connected. The "I am a fraud" feeling is creeping in, but I manage to push it back. 

I can't wait to be able to implement Node in future projects.