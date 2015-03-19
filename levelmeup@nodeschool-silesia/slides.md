class: center, middle

![Logo](images/nodeschool-silesia.png)

# NodeSchool - *levelmeup*

[nodeschool.io/silesia](http://nodeschool.io/silesia), [@nodeschoolpl](https://twitter.com/nodeschoolpl)

[@rspective](https://twitter.com/nodeschoolpl)

[![rspective](images/rspective.png)](http://blog.rspective.com)

???

- Cześć!
- Witamy w grupie, która zajmować się będzie nauką obsłgi bazy danych LevelDB.
- Korzystać będziemy z warsztatu *levelmeup*.

---

# Let's start!

1. [https://nodejs.org](https://nodejs.org)
2. `node -v`
3. `npm -v`
4. `npm install -g levelmeup`
5. `levelmeup`

???

- Jak zainstalować warsztat?
- Oto kilka kroków jakie należy wykonać, aby go zainstalować.

---

### Workshopper

![Workshopper Screen](images/workshopper.png)

???

- Oto jak wygląda ekran startowy, tuż po otwarciu warsztatu.
- Poruszamy się za pomocą kursorów, ENTER wybiera aktywną lekcję.
- Do każdego ćwiczenia dołączony jest opis zadania wraz z krótkim wprowadzeniem.

---

# LevelDB

1. LevelDB to prosta, open-source-owa baza typu klucz wartość zbudowana przez Google.
2. Jest używana m.in. w Chromie (IndexedDB) oraz innych produktach (Bitcoin).
3. Zapisuje posortowane po kluczach dane na dysku.
4. Kompresuje dane używając szybkiego algorytmu Snappy.
5. Wspiera:
   - dowolne tablice byte-ów jako klucze i wartości,
   - operacje GET, PUT, DELETE,
   - batchowe operacje PUT, DELETE,
   - dwukierunkowe iterowanie.
6. Minusy:
   - nie jest to baza SQL,
   - tylko jeden proces może mieć do niej dostęp naraz,
   - nie ma wsparcia dla modelu client-server.

![leveldb](images/leveldb.png)

???

- Zaczym zaczniemy - kilka zdań wprowadzenia na temat bazy LevelDB.
- LevelDB to prosta, open-source-owa baza typu klucz wartość zbudowana przez Google.
- Jest używana m.in. w Chromie (IndexedDB) oraz innych produktach (Bitcoin).
- Zapisuje posortowane po kluczach dane na dysku. Można dostarczyć customową funkcję sortującą.
- Kompresuje dane używając szybkiego algorytmu Snappy.

- Wspiera:
-- dowolne tablice byte-ów jako klucze i wartości,
-- operacje GET, PUT, DELETE,
-- batchowe operacje PUT, DELETE,
-- dwukierunkowe iterowanie.

- Minusy:
-- nie jest to baza SQL - nie wspiera relacji, nie wpiera zapytań SQL,
-- tylko jeden proces może mieć do niej dostęp naraz,
-- nie ma wsparcia dla modelu client-server - nie możemy jej wystawić jako serwer bez własnego wrappera.

---

# LevelUP

1. LevelUP to wrapper LevelDB dla Node.js.
2. Wspiera m.in. takie encodowania jak Buffer i JSON.
3. Iteratory oraz operacje batchowe mogą być używane jako stream-y.

```javascript
var levelup = require('levelup');
var db = levelup('./mydb'); // options, anync

db.put('name', 'LevelUP', function (err) {
  if (err) return console.error('Ooops!', err);

  db.get('name', function (err, value) {
    if (err) return console.error('Ooops!', err);

    console.log('name=' + value);
  });
});
```

???

- LevelUP to wrapper LevelDB dla Node.js.
- Wspiera m.in. takie encodowania jak Buffer i JSON.
- Iteratory oraz operacje batchowe mogą być używane jako streamy.

---

# LevelDOWN and others...

1. LevelDOWN to bindowanie pomiędzy LevelUP a LevelDB.
2. Zaczynając od wersji 0.9, LevelUP nie dostarcza już LevelDOWN poprzez swoje zależności.
3. Programista musi sam ją dostarczyć, lub jakąś alternatywę (level.js, MemDOWN).
4. Level to odrębny wrapper (jak LevelUP), który dostarcza LevelDOWN.

???

- LevelDOWN to bindowanie pomiędzy LevelUP a LevelDB. LevelUP używa LevelDOWN aby wykonywać operacje na LevelDB.
- Zaczynając od wersji 0.9, LevelUP nie dostarcza już LevelDOWN poprzez swoje zależności.

- Programista musi sam ją dostarczyć, lub jakąś alternatywę.
-- Level.js to implementacja LevelDB w przeglądarce.
-- MemDOWN to pamięciowa implementacja LevelDB.

- Level to odrębny wrapper (jak LevelUP), który dostarcza LevelDOWN w swoich zależnościach.
-- Powstał głównie aby ułatwić używanie LevelDB.
-- Tworzą go prawie ci sami ludzie. Level ma o jednego kontrybutora mniej.

---

# Code snippets

```javascript
var ops = [
    { type: 'del', key: 'old' },
    { type: 'put', key: 'new', value: 'Clown' }
]

db.batch(ops, function (err) {
  if (err) return console.log('Ooops!', err);
  console.log('Success!');
});
```

```javascript
db.batch()
  .del('old')
  .put('new', 'Clown')
  .write(function(err) {
     if (err) return console.log('Ooops!', err);
     console.log('Success!');
   })
```

```javascript
db.createReadStream() // options
  .on('data', function(data) {
    console.log(data.key, '=', data.value);
  })
  .on('error', function(err) {
    console.error('Ooops!', err);
  })
  .on('close', function() {
    console.log('Stream closed');
  })
  .on('end', function() {
    console.log('Stream closed');
  })
```

---
class: center, middle

# Let's get the hands *dirty* !