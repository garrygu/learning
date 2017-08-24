

### uppercase/lowercase
```
<p>{{ 'hello world' | uppercase }}</p>
```

### number
Syntax:   
`expression | number[:digitInfo]`  
`digitInfo`: `{minIntegerDigits}.{minFractionDigits}-{maxFractionDigits}`

### percent
Syntax:  
`expression | percent[:digitInfo]`

### currency
**Syntax**  
`expression | currency[:currencyCode[:symbolDisplay[:digitInfo]]]`  
`currencyCode`: the ISO 4217 currency code   
`symbolDisplay`: Boolean, default: false. Whether to use the currency symbol (for example, $) or the currency code (for, example USD) in the output  

**Examples**  
```
<p>{{ 11256.569 | currency:"GBP":true:'4.1-2' }}</p>
<!-- output is '£11,256.57' -->
```

### slice
Syntax:  
`expression | slice:start[:end]`  
Both `start` and `end` arguments can take positive and negative values, as the JavaScript `slice()` methods do.

### date
https://angular.io/api/common/DatePipe  

**Syntax**  
`expression | date[:format]`  
`expression`: must be a date object or a number (milliseconds since the UTC epoch)   

**Format**    
<table>
<tr><td>medium</td><td>equivalent to 'yMMMdjms' (for example, Sep 3, 2010, 12:05:08 PM for en-US)</td>
<tr><td>short</td><td>equivalent to 'yMdjm' (for example, 9/3/2010, 12:05 PM for en-US)</td>
<tr><td>fullDate</td><td>equivalent to 'yMMMMEEEEd' (for example, Friday, September 3, 2010 for en-US)</td>
<tr><td>longDate</td><td>equivalent to 'yMMMMd' (for example, September 3, 2010)</td>
<tr><td>mediumDate</td><td>equivalent to 'yMMMd' (for example, Sep 3, 2010 for en-US)</td>
<tr><td>shortDate</td><td>equivalent to 'yMd' (for example, 9/3/2010 for en-US)</td>
<tr><td>mediumTime</td><td>equivalent to 'jms' (for example, 12:05:08 PM for en-US)</td>
<tr><td>shortTime</td><td>equivalent to 'jm' (for example, 12:05 PM for en-US)</td>
</table>

**Examples**  
Assuming dateObj is (year: 2015, month: 6, day: 15, hour: 21, minute: 43, second: 11) in the local time and locale is 'en-US':
```
{{ dateObj | date }}               // output is 'Jun 15, 2015'
{{ dateObj | date:'medium' }}      // output is 'Jun 15, 2015, 9:43:11 PM'
{{ dateObj | date:'shortTime' }}   // output is '9:43 PM'
{{ dateObj | date:'mmss' }}        // output is '43:11'
```

### JSON
```
{{ { name: 'Eve', age: 43 } | json }}
<!-- output：{ "name": "Eve", "age": 43 } -->
```

### replace
Syntax:  
`expression | replace:pattern:replacement`

### i18ns
#### i18nPlural
```
<h1> {{ pomodoros | i18nPlural:pomodorosWarningMapping }} </h1>

class MyPomodorosComponent {
  pomodoros: number;
  pomodorosWarningMapping: any = {
    '=0': 'No pomodoros for today',
    '=1': 'One pomodoro pending',
    'other': '# pomodoros pending'
  }
}
```

#### i18nSelect
```
<button (click)="togglePause()">
  {{ languageCode | i18nSelect: localizedLabelsMap }}
</button>

class PomodoroTimerComponent {
  languageCode: string = 'fr';
  localizedLabelsMap: any = {
    'en': 'Start timer',
    'es': 'Comenzar temporizador',
    'fr': 'Démarrer une séquence',
    'other': 'Start timer'
  }
  ...
}
```

### async
Subscribes to an observable or promise and returns the latest value it has emitted.
