# MyReactStudy

ReactJS로 영화 웹 서비스 만들기

---

## #1 INTRODUCTION

- React 사람들이 정말 많이 쓴대 !! 👏
- VScode Extension - Prettier: Code formatter
    
    shift + option + f
    

## #2 THE BASICS OF REACT

1. Before React
    - 바닐라JS
        
        ```html
        <!DOCTYPE html>
        <html>
            <body>
                <span>Total click: 0</span>
                <button id="btn">Click me!</button>
            </body>
            <script>
                let counter = 0;
                const button = document.getElementById("btn");
                const span = document.querySelector("span");
                function handleClick() {
                    counter = counter + 1;
                    span.innerText = `Total click: ${counter}`;
                }
                button.addEventListener("click", handleClick);
            </script>
        </html>
        ```
        
    - React JS 설치
        
        두 개의 JS코드 import
        
        ```html
        <html>
            <body></body>
            <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
            <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
        </html>
        ```
        
        React JS : 어플리케이션이 interactive하도록 만들어주는 라이브러리
        
        React-dom : 모든 React element들을 HTML body에 둘 수 있도록 해주는 라이브러리(패키지)
        
2. Our First React Element
    - html 태그 생성하기
        
        ```jsx
        const span = React.createElement("span"); 
        //첫번째 argument는 HTML 태그
        const span = React.createElement("span", { id: "j-span" });
        //두번째 argument는 property : class name, id 등
        const span = React.createElement(
        		"span",
        		{ id: "j-span", style: {color: "red"} },
        		"Hello I'm a span"
        );
        //세번째 argument는 content 
        //인자를 비울 때는 null
        ```
        
    - body 안에 React element 가져다 두기
        
        ```html
        <body>
            <div id="root"></div>
        </body>
        <script>
            const span = React.createElement("span"); 
            const root = document.getElementById("root");
            ReactDOM.render(span, root);
        </script>
        ```
        
        render : React element를 가지고 HTML로 만들어 배치하는 것
        
        바닐라JS와 달리, React JS는 JS에서 html 요소를 생성함.
        
        → React JS가 결과물인 HTML을 업데이트할 수 있다!
        
    1. Events in React
        - 한 번에 rendering
            
            ```jsx
            const container = React.createElement("div", null, [span, btn]);
            ReactDOM.render(container, root);
            ```
            
        - property 자리에 event listener 등록
            
            ```jsx
            const btn = React.createElement(
            		"button",
                {
                onClick: () => console.log("I'm clicked"),
                },
                "Click me"
            );
            ```
            
        
        → 사실 createElement()를 잘 사용하지는 않음 !!
        
    2. JSX
        - createElement를 대체할 수 있는 문법, JavaScript를 확장한 문법
            
            ```jsx
            //JSX 문법을 이용한 코드
            const Title = (
                  <h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
                    Hello I'm a title
                  </h3>
            );
            
            //createElement()를 이용한 코드
            const Title = React.createElement(
                  "h3",
                  { id: "title", onMouseEnter: () => console.log("mouse enter"), },
                  "Hello I'm a title"
            );
            ```
            
        - BABEL
            
            JSX로 적은 코드를 브라우저가 이해할 수 있는 형태로 바꿔주는 도구
            
            ```jsx
            <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
            ```
            
        - 한 태그를 다른 태그에 포함시키기
            
            Title을 div content로 포함시키기 위해서 화살표 함수로 바꿔줘야 한다.
            
            ```jsx
            const Title = () => (
                  <h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
                    Hello I'm a title
                  </h3>
             );
            const Container = <div> <Title/> <Btn/> </div>;
            //화살표 함수가 아니라면 Title을 string으로 인식
            //컴포넌트의 첫 글자는 반드시 대문자! ⭐️
            //<button> : html 태그, <Button> : 사용자 지정 태그
            //<Title/> : 함수형태, 동일한 코드 재사용 가능
            ```
            
            위와 같은 코드
            
            ```jsx
            function Title() {
            	return (
            		<h3 id="title" onMouseEnter={() => console.log("mouse enter")}>
                    Hello I'm a title
                </h3>
            	);
            }
            ```
            
        - 유의점
            
            for, class 등은 JavaScript 용어
            
            jsx는 HTML과 비슷하지만 다른 점 몇가지 기억할 필요가 있음 !!
            
            1) class → className
            
            2) for → htmlFor
            

## #3 STATE

1. Understanding State
    - State : 데이터가 저장되는 곳
    - innerText에 변수 담기 : { counter }
        
        ```jsx
        let counter = 0;
        const Container = (
           <div>
              <h3>Total clicks: {counter}</h3>
              <button>Click me</button>
           </div>
        );
        ```
        
    - html에 표시하는 것이 렌더링 !!
        
        ```jsx
        function countUp() {
            counter = counter + 1;
            ReactDOM.render(<Container/>, root);
        		//값이 업데이트 될 때마다 re-rendering 필요
         }
        ```
        
    
    → 자주 사용하지 않는 어려운 방법!
    
    → ReactJs를 제대로 활용하는 방법⭐️: UI에서 바뀐 부분만 업데이트! re-rendering X
    
2. setState
    - React.js 어플 내에서 데이터를 보관하고 자동으로 리렌더링 일으킬 수 있는 방법
        
        render() 함수를 호출하지 않고! 자동으로! UI에서 바뀐 부분만!
        
    - useState()
        
        ```jsx
        const [counter, modifier] = React.useState(0);
        //첫번째는 변수명, 두번째는 해당 변수를 변화시킬 함수명
        //보통은 modifier가 아니라 set(변수) 로 작명함. ex. setCounter
        
        //위와 같은 코드
        const counter = React.useState(0)[0];
        const modifier = React.useState(0)[1];
        ```
        
    - setCounter
        
        해당 값으로 업데이트하고 해당 부분만 re-rendering 시킴.
        
        ```jsx
        const onClick = () => {
           setCounter(counter + 1);
        }
        //counter 값도 자동으로 업데이트됨.
        ```
        
3. State Functions
    - setCounter에 함수를 넣을 수 있음!
        
        ```jsx
        setCounter(counter + 1);
        //직접 값을 설정해주는 방법은 비추!
        setCounter((current) => current + 1);
        //첫번째 argument는 현재 값
        //return값은 새로 갱신될 값
        //현재 state값을 바탕으로 다음 state값을 계산 -> 아래 방법
        ```
        
        둘 다 현재의 state를 가지고 새로운 값을 계산해냄
        
        → but, 두번째 방법이 더 안전
        
    - label
        
        ```html
        <div>
            <h1>Super Converter</h1>
            <label for="minutes">Minutes</label>
            <input id="minutes" placeholder="Minutes" type="number"/>
            <label for="hours">Hours</label>
            <input id="hours" placeholder="Hours" type="number"/>
        </div>
        ```
        
        input 입력칸 왼쪽에 표시되는 글자, 누르면 해당 입력칸으로 넘어감.
        
    - onChange
        
        input 태그의 값이 변경된 이벤트
        
        ```html
        <input
        value={minutes}
        id="minutes"
        placeholder="Minutes"
        type="number"
        onChange={onChange}
        />
        ```
        
        ```jsx
        const onChange = (event) => {
        		setMinutes(event.target.value);
        };
        ```
        
4. State Practice
    - 수식 값 보여주기
        
        ```html
        <input
          value={Math.round(minutes / 60)}
        />
        ```
        
        minutes만 re-render 되기 때문에 minutes 를 활용한 hours input 태그임.
        
    - flip
        
        클릭한 부분은 enabled(입력 가능)/ 그렇지 않은 부분은 disabled(입력 불가능)
        
        ```jsx
        const [flipped, setFlipped] = React.useState(false);
        const onFlip = () => setFlipped((current) => !current);
        
        <input
        		disabled={!flipped}
        />
        <input
        		disabled={flipped}
        		//disabled={flipped === false}
        />
        ```
        
    - 컴포넌트 분할 정복
        
        ```jsx
        function MinutesToHours() {
        			...
        };
        function App() {
            return (
                <div>
                  <h1>Super Converter</h1>
                  <MinutesToHours/>
        						//MinutesToHours에 있는 모든 요소들을 그려줌.
        						//재사용 가능. 함수형 컴포넌트
                </div>
            );
        };
        ```
        
        select 함수
        
        state를 변화시킬 때 모든 게 새로고침 된다.
        
        → React JS는 re-render가 필요한 것들을 re-render 시킨다.
        
        ```jsx
        function App() {
              const [index, setIndex] = React.useState("0");
              const onSelect = (event) => {
                setIndex(event.target.value);
              };
              return (
                <div>
                  <select value={index} onChange={onSelect}>
                    <option value="0">Minutes & Hours</option>
                    <option value="1">Km & Miles</option>
                  </select>
                  {index === "0" ? <MinutesToHours/> : <KmToMiles/>}
        					//중괄호 속의 코드는 string이 아닌 JS코드로 인식
                </div>
              );
        }
        ```
        
    - Super Converter 코드
        
        ```jsx
        <!DOCTYPE html>
        <html>
          <body>
            <div id="root"></div>
          </body>
          <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
          <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
          <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
          <script type="text/babel">
            const root = document.getElementById("root");
            function MinutesToHours() {
              const [amount, setAmount] = React.useState();
              const [flipped, setFlipped] = React.useState(false);
              const onChange = (event) => {
                setAmount(event.target.value);
              };
              const reset = () => setAmount(0);
              const onFlip = () => {
                reset();
                setFlipped((current) => !current);
              };
              return (
                <div>
                  <div>
                    <label htmlFor="minutes">Minutes</label>
                    <input
                      value={flipped ? amount * 60 : amount}
                      id="minutes"
                      placeholder="Minutes"
                      type="number"
                      onChange={onChange}
                      disabled={flipped === true}
                    />
                  </div>
                  <div>
                    <label htmlFor="hours">Hours</label>
                    <input
                      value={flipped ? amount : Math.round(amount / 60)}
                      id="hours"
                      placeholder="Hours"
                      type="number"
                      onChange={onChange}
                      disabled={flipped === false}
                    />
                  </div>
                  <button onClick={reset}>Reset</button>
                  <button onClick={onFlip}>Flip</button>
                </div>
              );
            }
        
            function KmToMiles() {
              const [input, setInput] = React.useState();
              const [flip, setFlip] = React.useState(false);
              const onChange = (event) => {
                setInput(event.target.value);
              };
              const reset = () => setInput(0);
              const onFlip = () => {
                reset();
                setFlip((current) => !current);
              };
              return (
                <div>
                  <div>
                    <label htmlFor="km">Km</label>
                    <input
                      value={flip ? input / 1000 : input}
                      id="km"
                      placeholder="Km"
                      type="number"
                      onChange={onChange}
                      disabled={flip}
                    />
                  </div>
                  <div>
                    <label htmlFor="miles">Miles</label>
                    <input
                      value={flip ? input : input * 1000}
                      id="miles"
                      placeholder="Miles"
                      type="number"
                      onChange={onChange}
                      disabled={!flip}
                    />
                  </div>
                  <button onClick={reset}>Reset</button>
                  <button onClick={onFlip}>Flip</button>
                </div>
              );
            }
        
            function App() {
              const [index, setIndex] = React.useState("0");
              const onSelect = (event) => {
                setIndex(event.target.value);
              };
              return (
                <div>
                  <h1>Super Converter</h1>
                  <select value={index} onChange={onSelect}>
                    <option value="0">Minutes & Hours</option>
                    <option value="1">Km & Miles</option>
                  </select>
                  {index === "0" ? <MinutesToHours /> : <KmToMiles />}
                </div>
              );
            }
            ReactDOM.render(<App />, root);
          </script>
        </html>
        ```
        

## #4 PROPS

1. Props
    - 부모 컴포넌트로부터 자식 컴포넌트에 데이터를 보낼 수 있게 해주는 방법
    - 호출하는 쪽에서 보낸 모든 정보를 가진 오브젝트
        
        ```jsx
        function Btn(props) {
              return (
                <button>
                  {props.myText} <---------------------------------------
                </button>
              )
        }
        function App() {
              return (
                <div>
                  <Btn myText="Jnary"/>
                  <Btn myText="Jin"/>
                </div>
              );
        }
        ```
        
    - Reusable!!
2. shortcut
    
    오브젝트로부터 property를 꺼내는 것
    
    ```jsx
    function Btn({myText, big}) {
          return (
            <button style={{fontSize: big ? 18 : 16,}}>
              {myText} <---------------------------------------
            </button>
          )
    }
    function App() {
          return (
            <div>
              <Btn myText="Jnary" big={true}/>
              <Btn myText="Jin"/>
    					//big 속성이 없으므로 속성값은 undefined
            </div>
          );
    }
    ```
    
3. function props
    
    전달해주는 인자는 just prop!
    
    ex. onClick을 전달한다고 이벤트 리스너 등록되는 것이 아님! 그저 prop의 이름일 뿐
    
    ```jsx
    function Btn({text, onClick}) {
          return (
            <button onClick={onClick}>
              {text}
            </button>
          )
    }
    function App() {
          const [value, setValue] = React.useState("Save Changes");
          const changeValue = () => setValue("Revert Changes");
          return (
            <div>
              <Btn text={value} onClick={changeValue}/>
    					<Btn text="Jnary"/>
            </div>
          );
    }
    ```
    
    줄이기
    
    ```jsx
    function Btn({fontSize}) {
    		return (
    			<button
    				fontSize : fontSize,
    				//이름이 같으므로 아래와 같이 줄일 수 있음.
    				fontSize,  
    			>
    			</button>
    }
    ```
    
4. Memo
    - 부모 자식 컴포넌트 re-render
        
        1번 Btn, 2번 Btn 둘 다 re-render된다. → 2번의 불필요한 re-rendering
        
        부모 컴포넌트의 어떤 state가 변경된다면, 모든 자식들이 re-render 된다.
        
        → 추후에 어플리케이션이 느려지는 원인이 될 수 있음
        
    - React Memo
        
        props가 변경되지 않는다면 다시 그릴 필요가 없다는 것을 명시해주는 것
        
        props가 변경된 것만 re-render시킴
        
    
    ```jsx
    const MemorizedBtn = React.memo(Btn);
    function App() {
          ...
    return (
        <div>
            <MemorizedBtn text={value} onClick={changeValue}/>
            <MemorizedBtn text="Jin"/>
        </div>
    ```
    
5. Prop Types
    - 설치
        
        ```jsx
        <script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>
        ```
        
    - 내가 어떤 타입의 prop을 받고 있는 지 체크
        
        ```jsx
        Btn.propTypes = {
           text: propTypes.string,
           fontSize: propTypes.number,
        }
        ```
        
    - 종류
        
        array, bool, func, number, object, string, symbol, node, element,,
        
        ```jsx
        //둘 중 하나만 유효
        PropTypes.oneOf(['News', 'Photos']),
        
        //작명 option-, 
        optionFunc: PropTypes.func,
        requiredAny: PropTypes.any.isRequired,
        ```
        
    
    → 에러를 표시해주지만 잘 작동됨.
    

## #5 CREATE REACT APP

1. create-react-app
    
    React 어플리케이션을 위해 많은 스크립트들과 많은 사전 설정들을 준비해줌.
    
    장점 : Auto-Reload (자동 재실행)
    
    - 시작
        
        ```bash
        >> npx create-react-app react-movie-app
        ```
        
    - 개발용 서버 만들기
        
        ```bash
        >> npm start
        ```

    - propTypes 설치하기
        
        ```jsx
        >> npm i prop-types
        ```
        
        ```jsx
        import PropTypes from "prop-types";
        ```
        
    - 폴더
        
        src 폴더 : 모든 파일들을 넣을 폴더
        
        ⭐️src/index.js : src에 있는 우리 코드를 가져다가 페이지 어딘가로 넣어줌
        
        App.js, index.js만 남겨두면 빈 프로젝트 완성!
        
2. Tour of CRA
    - export/import
        
        ```jsx
        export default Button;
        //외부에서 컴포넌트 사용할 수 있게 내보내기
        
        import Buttom from "./Button";
        //Button.js 파일에서 컴포넌트 가져오기
        ```
        
    - CSS modules
        
        파일 분할 정복 → 특정 컴포넌트를 위한 CSS 파일 생성 기능 획득!
        
        ```css
        // Button.module.css 파일
        .btn {
            color: white;
            background-color: tomato;
        }
        ```
        
        ```jsx
        import styles from "./Button.module.css";
        function Button({ text }) {
          return <button className={styles.btn}>{text}</button>;
        }
        ```
        

## #6 EFFECTS ⭐️

1. 효과
    - 첫 번째 render에만 코드가 실행되고, 다른 state 변화에는 실행되지 않도록 설정할 때 사용
    - state 변경 → 모든 code들 다시 실행
2. useEffect
    - 두 개의 argument
        
        1) 우리가 실행시키고 싶은 코드 React.EffectCallback
        
        2) 변화를 지켜봐야하는 리스트 React.DependencyList
        
    - 첫 번째 argument
        
        ```jsx
        import {useEffect} from "react";
        
        useEffect(() => {
            console.log("CALL THE API ...");
        }, [])
        //코드가 딱 한 번만 실행할 수 있게 보호해줌.
        ```
        
    - 두 번째 argument
        
        ```jsx
        const [keyword, setKeyword] = useState("");
        useEffect(() => {
          console.log("Search for", keyword);
        }, [keyword]);
        //keyword가 변화할 때만 코드를 실행할 수 있도록
        ```
        
        ```jsx
        useEffect(() => {
          console.log("Search for", keyword);
        }, [keyword, count]);
        //여러 개 가능
        ```
        
3. Cleanup
    - component가 destroy될 때 동작하는 function
        
        ```
        function Hello() {
          useEffect(() => {
            console.log("created :)");
            return () => console.log("destroyed :(")
        		//이게 Cleanup function
          }, []);
          return <h1>Hello</h1>;
        }
        ```
        
    - useEffect 첫 번째 argument가 해당 함수를 리턴해야함!

## #7 PRACTICE MOVIE APP

1. To Do List
    - useState 리스트 추가하기
        
        ```jsx
        const order = [1, 2, 3]
        [6, ...order]
        //배열 element 모두 가져오는 문법
        >> [6, 1, 2, 3]
        ```
        
        ```jsx
        const [toDo, setToDo] = useState("");
        const [toDos, setToDos] = useState([]);
        const onSubmit = (event) => {
          event.preventDefault();
          if (toDo === "") {
              return;
          }
          setToDo("");
          setToDos((currentArray) => [toDo, ...currentArray]);
        }
        ```
        
    - map
        
        ```jsx
        ['a', 'b', 'c', 'd'].map((item) => item.toUpperCase())
        >> ['A', 'B', 'C', 'D']
        ```
        
        첫 번째 argument : value
        
        두 번째 argument : index (key)
        
        ```jsx
        <ul>
          {toDos.map((item, index) => (
            <li key={index}>{item}</li>
          ))}
        </ul>
        ```
        
2. 예제 연습하기 : Coin Tracker
    - api 이용 → useEffect
        
        [https://api.coinpaprika.com/v1/tickers](https://api.coinpaprika.com/v1/tickers)
        
    - fetch → Loading… 표시
        
        ```jsx
        const [loading, setLoading] = useState(true);
        const [coins, setCoins] = useState([]);
        //참조했을 때 undefined가 되지 않도록 빈 array라도 둠.
        useEffect(() => {
          fetch("https://api.coinpaprika.com/v1/tickers")
            .then((response) => response.json())
            .then((json) => {
              setCoins(json);
              setLoading(false);
            });
        }, []);
        ```
        
    - 리스트
        
        ```jsx
        <ul>
          {coins.map((coin) => (
            <li>
              {coin.name} ({coin.symbol}) : {coin.quotes.USD.price} USD
            </li>
          ))}
        </ul>
        ```
        
    - 옵션
        
        ```jsx
        <select>
          {coins.map((coin) => (
            <option>
              {coin.name} ({coin.symbol}) : {coin.quotes.USD.price} USD
            </option>
          ))}
        </select>
        ```
        
3. Movie App
    - 영화 API
        
        [https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year](https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year)
        
    - async-await
        
        async 함수 내부에서만 사용 가능
        
        ```jsx
        //기존
        const [loading, setLoading] = useState(true);
        const [movies, setMovies] = useState([]);
        useEffect(() => {
          fetch(
            "https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year"
          )
            .then((response) => response.json())
            .then((json) => {
               setMovies(json.data.movies);
               setLoading(false);
             });
        }, []);
        ```
        
        ```jsx
        //async-await 사용 (보통 이렇게 많이 함)
        const getMovies = async() => {
          const response = await fetch(
            "https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year"
          );
          const json = await response.json();
          setMovies(json.data.movies);
          setLoading(false);
        }
        useEffect(() => {
          getMovies();
        }, []);
        ```
        
        ```jsx
        //짧은 버전
        const getMovies = async () => {
          const json = await (
            await fetch(
              "https://yts.mx/api/v2/list_movies.json?minimum_rating=9&sort_by=year"
            )
          ).json();
        ```
        
    - 컴포넌트 분리
        - 기존
            
            ```jsx
            return (
              <div>
                {loading ? (
                  <h1>Loading...</h1>
                ) : (
                  <div>
                    {movies.map((movie) => (
                      <div key={movie.id}>
                        <img src={movie.medium_cover_image}/>
                        <h2>{movie.title}</h2>
                        <p>{movie.summary}</p>
                        <ul>
                          {movie.genres.map((g) => (
                            <li key={g}>{g}</li>
                          ))}
                        </ul>
                      </div>
                    ))}
                  </div>
                )}
              </div>
            );
            ```
            
        
        ```jsx
        //App.js
        import Movie from "./Movie";
        return (
          <div>
            {loading ? (
              <h1>Loading...</h1>
            ) : (
              <div>
                {movies.map((movie) => (
                  <Movie
        						key={movie.id}
        						//key는 map 안에서 component를 render할 때 사용
                    medium_cover_image={movie.medium_cover_image}
                    title={movie.title}
                    summary={movie.summary}
                    genres={movie.genre}
                  />
                ))}
              </div>
            )}
          </div>
        );
        ```
        
        ```jsx
        //Movie.js
        function Movie({ medium_cover_image, title, summary, genres }) {
          return (
            <div>
              <img src={medium_cover_image alt={title}} />
        			//모든 이미지 element들은 alt속성을 가짐.
              <h2>{title}</h2>
              <p>{summary}</p>
              <ul>
                {genres.map((g) => (
                  <li key={g}>{g}</li>
                ))}
              </ul>
            </div>
          );
        }
        
        export default Movie;
        ```
        
    - PropType 이용하기
        
        ```jsx
        Movie.propTypes = {
            medium_cover_image: PropTypes.string.isRequired,
            title: PropTypes.string.isRequired,
            summary: PropTypes.string.isRequired,
            genres: PropTypes.arrayOf(PropTypes.string).isRequired,
        }
        ```
        
4. React Router
    - React Router
        
        스크린 단위로 페이지 전환하는 것
        
        ```jsx
        >> npm install react-router-dom
        ```
        
    - App.js는 router를 render한다.
        
        → Url에 따라 보여지는 스크린이 달라짐!

        ```jsx
        //App.js
        import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";
        
        import Home from "./routes/Home";
        import Detail from "./routes/Detail"
        
        function App() {
          return (
            <Router>
              <Switch>
                <Route path="/movie">
                  <Detail/>
                </Route>
                <Route path="/">
                  <Home />
                </Route>
              </Switch>
            </Router>
          );
        }
        export default App;
        ```
        
    - Router 종류
        - BrowserRouter : localhost:5000/movie
        - HashRouter : localhost:5000/#/movie
    - Switch
        
        한 번의 하나의 Route만 렌더링 하기 위해 사용
        Router에서는 두 개의 Route를 한 번에 렌더링 가능
        
    - Link
        
        브라우저 새로고침 없이도 유저를 다른 페이지로 이동시켜주는 컴포넌트
        
        ```jsx
        //Movie.js
        <h2>
          <Link to="/movie">{title}</Link>
        </h2>
        ```
        
5. Parameters
    - Dynamic Url
        
        url에 변수를 넣을 수 있다!
        
        ex. localhost:5000/movie/921
        
        ```jsx
        //App.js
        <Route path="/movie/:id">
        //id에 원하는 무엇이든 넣을 수 있다.
        ```
        
        ```jsx
        //Movie.js
        <h2>
          <Link to={`/movie/${id}`}>{title}</Link>
        </h2>
        ```
        
    - useParams
        
        React-router에서 제공하는 url에 있는 값을 반환해주는 함수
        
        ```jsx
        import {useParams} from "react-router-dom";
        const x = useParams();
        console.log(x);
        
        >> {id: 921}
        //Route에 써둔 변수명과 변수값 그대로 가져옴.
        ```
        
        ```jsx
        const {id} = useParams();
        console.log(id);
        
        >>921
        ```
        
6. Publishing
    - Github pages
        
        html, css, javascript를 올리면 웹사이트로 만들어서 전세계에 무료로 배포해주는 서비스
        
        ```jsx
        npm i gh-pages
        //github pages에 업로드 할 수 있게 해주는 패키지 
        ```
        
    - Production ready
        
        코드 압축 및 최적화
        
        → 브라우저가 이해할 수 있는 코드로 최적화
        
        ```jsx
        npm run build
        //웹사이트의 production ready code 생성
        //build 폴더 안에 최적화된 코드를 저장
        ```
        
    - deploy
        
        gh-pages를 실행시키고 ‘build’라는 디렉토리를 가져감.
        
        → homepage에 적힌 웹사이트에 업로드
        
        ```jsx
        //package.json
        "scripts": {
            "deploy": "gh-pages -d build",
            "predeploy": "npm run build"
        		//npm run deploy하면 위의 두개가 실행됨.
        },
        "homepage": "http://j-nary.github.io/MyReactStudy"
        ```
        
7. Next Steps
    - Breaking change
        
        버전을 업데이트하면서 코드가 깨져서 코드를 수정해야하는 경우 😡
        
        → React.js 는 그런 경우 없음 !