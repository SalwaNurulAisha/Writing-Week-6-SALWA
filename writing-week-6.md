### REACT REDUX

- salah satu library/ package/ framework yang ukurannya sangat kecil.
- punya learning curve yang tinggi sehingga sangat sulit dan membuat pusing.
- Redux bisa di combine dengan react, vue, angular.
- **SIFAT REDUX**

  - **_Flexible_** : bisa pakai UI layer apa saja.
  - **_Predictable_** : punya satu buah arus yang searah, ketika ui berubah langsung ke action => reducer => store => kembali ke ui.
  - **_Centralized_** : dapat memusatkan state dan logic aplikasi agar memiliki kemampuan yang kuat untuk melakukan undo / redo (membatalkan / mengulangi sesuatu yg telah dibatalkan), persitensi state, dll
  - **_Debuggable_** : memudahkan pelacakan kapan, di mana, mengapa, dan bagaimana status aplikasi berubah serta mengirim laporan kesalahan lengkap ke server.

- **REDUX STORE**

  - tempat menaruh state.
  - **Tujuannya** : manahan state agar aman.
  - **Prinsipnya** :

    1. Redux principle 1

       - punya satu buah single of truth yaitu sebuah aplikasi ditaruh ke dalam satu buah tempat penyimpanan state yg sama.
       - satu buah state object yang dapat memanage satu buah penyimpanan
       - sebuah action hanya sebuah perintah, yang menjalankan adalah sistem.

    2. Redux principle 2

       - state hanya boleh dibaca, tidak untuk diubah, jika ingin mengubah menggunakan **_reducer_**.

    3. Redux principle 3

       - untuk dapat melakukan sebuah perubahan harus menuliskan pure reducer (perintah valid dan terverifikasi).
         > **_dispatch_** : untuk mengirimkan sesuatu yaitu action yang sudah terverifikasi agar bisa dilakukan.
         > **contohnya :** surat jalan untuk melakukan perubahan data pada store.

- **FLOW REDUX**

  - user(react library) => perintah(action) => cashier(reduser) => vault(store) => user

- **YANG PERLU DISIAPKAN UNTUK MEMBUAT REDUX**

  - menginstall redux
    ```js
        npm install react-redux axios @reduxjs/toolkit
    ```
  - membuat store => ruang tempat penyimpanan data.
  - reducer / slice => wadah tempat penyimpanan data dalam ruang.
  - action => pengkondisian data yang berada dalam penyimpanan wadah. (tambah, kurang, ganti)
  - bungkus app => profite bahwa semua data di dalam ruang penyimpanan dapat diakses oleh komponen manapun.
  - useSelector => pengambilan data

- **LANGKAH - LANGKAH RUNTUT MEMBUAT REDUX**
  :one: membuat store dan memasukan ke index.js folder src.

  - membuat folder store yang didalamnya terdapat file index.js
    ![store](/assets/store.PNG)
    - dalam index.js mengimport configureStore
      ```js
      import { configureStore } from "@reduxjs/toolkit";
      ```
    - kemudian buat function
      ```js
      const store = configureStore({
        reducer: {
          tes: "test",
        },
      });
      export default store;
      ```
  - import provider dari react redux dan memasukan store ke dalam provider dalam file index.js folder src
    ![index src](/assets/index%20src.PNG)

    - mengimport provider

    ```js
    import { Provider } from "react-redux";
    ```

    - mengimport store

    ```js
    import store from "./store";
    ```

    - masukan store ke provider

    ```js
    const root = ReactDOM.createRoot(document.getElementById("root"));
    root.render(
      <React.StrictMode>
        <Provider store={store}>
          <App />
        </Provider>
      </React.StrictMode>
    );
    ```

  :two: Membuat komponen view dan slice.
  ![componets view dan slice](/assets/component%20view%20%26%20slice.PNG)

  - Pertama-tama isi BookView.js dengan teks aja

    ```js
    const BooksView = () => {
      return <div>BooksView</div>;
    };

    export default BooksView;
    ```

  - isi BooksSlice.js dengan createSlice untuk reducer yang dikirim ke store

    - mengimport createSlice
      ```js
      import { createSlice } from "@reduxjs/toolkit";
      ```
    - buat function initialState
      ```js
      const initialState = {
        totalBooks: 123,
        totalAuthor: 50,
      };
      ```
    - buat createSlice untuk reducer yang dikirim ke store

      ```js
      const bookSlice = createSlice({
        name: "book",
        initialState,

        //REDUCERS
        reducers: {
          //ACTION BORROW
          borrow: (state) => {
            state.totalBooks--;
          },

          plusAuthor: (state) => {
            state.totalAuthor += 100;
          },
        },
      });
      ```

    - export function slice reducer dan actionnya
      ```js
      export default bookSlice.reducer;
      export const { borrow, plusAuthor } = bookSlice.actions;
      ```

  - Store menerima createSlice dari BooksSlice - import createSlice reducer dari BookSlice
    ```js
    import bookReducer from "../components/Books/BooksSlice";
    ```
    - masukkan createSlice reducer dari BookSlice
    ```js
    const store = configureStore({ reducer: { book: bookReducer } });
    export default store;
    ```

  :three: Memunculkan state dari store redux.

  - gunakan useSelector dalam komponen BookView.js

    - import useSelector

    ```js
    import { useSelector } from "react-redux";
    ```

    - buat function untuk munculkan state menggunakan useSelector

    ````js
    const BooksView = () => {
    const totalRedux = useSelector((state) => state.book);

              return (
                  <div>
                    <div>BooksView</div>
                    <h1>Total Books: {totalRedux.totalBooks}</h1>
                    <h1>Total Author: {totalRedux.totalAuthor}</h1>
                  </div>
              );
            };
           ```

    :four: Mengubah state di BookView.js dari store redux menggunakan action.

    ````

  - import action dari bookSlice
    ```js
    import { borrow, plusAuthor } from "./BooksSlice";
    ```
  - Buat button untuk melakukan useDispatch action

    ```js
    import { useSelector, useDispatch } from "react-redux";
    import { borrow, plusAuthor } from "./BooksSlice";

    const BooksView = () => {
      const totalRedux = useSelector((state) => state.book);

      console.log("total", totalBooksRedux);

      // function dispatch
      const dispatch = useDispatch();

      return (
        <div>
          <div>BooksView</div>
          <h1>Total Books: {totalRedux.totalBooks}</h1>
          <h1>Total Author: {totalRedux.totalAuthor}</h1>
          // Button useDispatch
          <button onClick={() => dispatch(borrow())}>Borrow</button>
          <button onClick={() => dispatch(plusAuthor())}>Plus Author</button>
        </div>
      );
    };

    export default BooksView;
    ```

### REDUX THUNK

- standar penulisan async logic di redux.
- fungsinya untuk mengambil data.
- Contoh penggunaan redux thunk
  ![Redux Thunk](/assets/redux%20thunk.png)

### REACT CONTEXT

- **Cara kerja** : bisa menambahkan data dari store secara lompat-lompat, tanpa harus berurutan seperti redux.
- jika menggunakan context ga perlu install apapun.
- **Langkah menggunakan context**

  1. membuat folder context yang didalamnya terdapat file UserContext.js (nama file menyesuaikan) dalam folder src
     ![context](/assets/context.PNG)
  2. pada file UserContext.js, import useState dan createContext tanpa perlu menginstal terlebih dahulu.

     ```js
     import { useState, createContext, useEffect } from "react";
     import axios from "axios";
     ```

  3. buat setup context dengan createContext.
     ```js
     export const UserContext = createContext();
     ```
  4. definisikan isi data context yg akan tersedia dalam komponen provider.
     ![isi data context](/assets/isi%20userContext.png)
  5. buat folder components dengan isi file User.js
     ![component user.js](/assets/component%20user.PNG)
  6. import UserContext ke dalam file User.js
     ```js
     import { UserContext } from "../context/UserContext";
     ```
  7. tampilkan data UserContext ke dalam halaman user yang akan tersedia dalam komponen consumer.
     ![isi User.js](/assets/isi%20User.js.png)
  8. bungkus context provider ke bagian paling luar di dalam return App.js

     ```js
     import "./App.css";

     import User from "./components/User";

     import UserContextProvider from "./context/UserContext";

     function App() {
       return (
         <div className="App">
           <UserContextProvider>
             <h1>React Context</h1>
             <User />
           </UserContextProvider>
         </div>
       );
     }

     export default App;
     ```

### REACT TESTING

- npm run test => untuk melakukan testing.
- nama testing bebas, seperti commit, tapi harus menggambarkan apa yg sedang di tes.
- pada pemanggilan testing setiap karakter harus sesuai, tidak ada toleransi sedikitpun.
