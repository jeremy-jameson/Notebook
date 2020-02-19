# TrimCss

Thursday, April 05, 2012
6:13 AM

```C#
using System;
using System.IO;
using System.Text;

namespace TrimCss
{
    class Program
    {
        static void Main(string[] args)
        {
            string inputFile =
@"C:\\NotBackedUp\\Fabrikam\\Demo\\Main\\Source\\Web\\PublishingLayouts\\Themes\\Theme-1.0\\Fabrikam-COREV4.css";

            string css = File.ReadAllText(inputFile);

            StringBuilder buffer = new StringBuilder();

            foreach (char c in css)
            {
                buffer.Append(c);

                if (c == '}')
                {
                    string block = buffer.ToString();

                    if (block.Contains(
                        "font-family",
                        StringComparison.OrdinalIgnoreCase) == true
                        || block.Contains(
                            "Dark1-",
                            StringComparison.OrdinalIgnoreCase) == true)
                    {
                        Console.Write(block);
                    }

                    buffer = new StringBuilder();
                }
            }
        }
    }

    static class StringExtensions
    {
        public static bool Contains(
            this string source,
            string toCheck,
            StringComparison comp)
        {
            return source.IndexOf(toCheck, comp) >= 0;
        }
    }

}
```
