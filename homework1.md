
## ЗАДАЧА №1
#### Код:

```css public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
} 
```
#### Память:
![image](https://user-images.githubusercontent.com/101247948/169232537-88122903-1015-4e95-b464-0766baed76a1.png)
</br>
</br>
</br>
#### Что происходит:
ClassLoader загружает класс JvmComprehension.  В стеке выделяется фрейм для метода main().
1. В **строке 1** в стеке создается int i = 1.  
2. ClassLoader загружает класс Object. В куче выделяется память для new Object(). В стеке **o** присваивает значение ссылки на Object из кучи.
3. ClassLoader загружает класс Integer. В куче выделяется память для Integer = 2. В стеке ii присваивается ссылка на Integer = 2 из кучи.
4. Выделяется новый фрейм для метода printAll(). Параметрам присваиваются ссылки и значения:
* **о** присваивает ссылку на Object из кучи, созданный в п.2
* создается переменная int i = 1
* ii присваиваются ссылка на Integer = 2 из пункта 3 
5. В куче создается объект Integer = 700. В стеке переменной uselessVar присваивается ссылка на него.
6. ClassLoader загружает класс System. В стеке создается новый фрейм для метода println(o.toString() + i + ii). В этом фрейме создается свой int i = 1, ii со ссылкой на объект Integer = 2 из пункта 3, **o** со ссылкой на объект Object из пункта 2. </br>
Создается еще один фрейм для метода toString(), после его выполнения фрейм удаляется. ClassLoader загружает класс String. *(Если смотреть через Debug, то создаются фреймы для методов print(), write(), ensureOpen(), min(), getChars(), checkBoundsBeginEnd(), checkBoundsOffCount() и прочие, фреймы создаются и удаляются, многие классы загружаются: PrintStream, Writer, BufferedWriter, StringLatin1 и проч).*</br>
Удаляется фрейм метода println(o.toString() + i + ii). Удаляется фрейм метода printAll().
7. В стеке создается новый фрейм для метода println("finished"). В куче создается новый объект строка "finished". </br>
Фрейм для метода println("finished") удаляется.</br>
Фрейм для метода main() удаляется. 




