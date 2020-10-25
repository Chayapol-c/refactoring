# Refactoring Example

## FlashGet
From Chayapol's Multi-Thread Downloader code: https://github.com/Chayapol-c/PA4-Chayapol

### convertByte() Method
In the `src/flashget/OutputFormat.java ` class

https://github.com/Chayapol-c/PA4-Chayapol/blob/master/src/flashget/OutputFormat.java

consider this code :
```java
        public String convertByte(double value) {
            if (value >= 1.0E9) return String.valueOf(value / 1.0E9) + " GB";
            else if (value >= 1.0E6) return String.valueOf(value / 1.0E6) + " MB";
            else if (value >= 1.0E3) return String.valueOf(value / 1.0E3) + " KB";
            return String.valueOf(value);
        }
```
* Refactoring Sign:

    * Magic numbers found which are 1.0E9, 1.0E6 and 1.0E3.
    * use return statement more than 3.

* Refactoring: create Enum for store values.

```java
    public Enum ByteUnit{
        NOUNIT(1.0,""),
        KB(1.0E3, " KB"),
        MB(1.0E6, " MB"),
        GB(1.0E9, " GB");

        private final double value;
        private final String name;

        private ByteUnit(double value, String name){
            this.value = value;
            this.name = name;
        }

        public String getTextValue(double memory) {
            return String.valueOf(memory/this.value) + this.name;
        }
        public double getValue(double value){
            return this.value
        }
    }

```
* Refactoring: create and call getTextValue() in convertByte().
```java
        public String convertByte(double value) {
            ByteUnit unit = ByteUnit.NOUNIT;
            if (value >= ByteUnit.GB.getValue()) unit = ByteUnit.GB;
            else if (value >= ByteUnit.MB.getValue()) unit = ByteUnit.MB;
            else if (value >= ByteUnit.KB.getValue()) unit  = ByteUnit.KB;
            return unit.getTextValue(value);
        }
```

