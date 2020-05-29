# Movie App 2019

React JS Fundamentals Course (2019 Update!)


src/routes/Home(rating 순으로 영화 정보를 보여주는 페이지)

컴포넌트를 생성 할 때는 
constructor -> componentWillMount -> render -> componentDidMount순으로 진행 된다.

JavaScript의 삼항연산자
--> isLoading ? (Loading...) : movies정보(isLoading이 true이면 Loading...을 보여주고 false이면 영화정보들을 화면에 뿌려준다.)

{movies.map(movie=> (
                <Movie 
                  key={movie.id}
                  id={movie.id} 
                  year={movie.year} 
                  title={movie.title} 
                  summary={movie.summary} 
                  poster={movie.medium_cover_image}
                  genres={movie.genres}
                />
              ))}
--> map()은 movies배열을 돌면서 하나의 item씩 실행한다. 여기서 하나의 item은 movie 이고 각 movie마다 <Movie ... /> 컴포넌트가 그려진다.
key,id,year,title,summary,poster,genres는 Movie컴포넌트로 보내는 props들이다.

async componentDidMount(){
    const {
      data:{
        data:{movies}
      }
    } = await axios.get("https://yts-proxy.now.sh/list_movies.json?sort_by=rating");
    this.setState({movies, isLoading: false})
  }
--> componentDidMount() 위에서 언급한대로 render()가 일어난 후 실행된다.
async, await은 비동기 프로그래밍인 자바스크립트에게 (async, await)을 기다리라고 한다. 그러므로 fetch를 쓰지않고 대신에 axios.get(URL)을 movies에 넣을 때 까지 기다리고! this.setState(movies에 movies를 넣고, isLoading을 false로 설정)실행한다.

--------------------------------------------------

src/components/Movie(영화정보를 담고있는 컴포넌트)

해체할당자
--> props를 받을 때 기본적으로 Object형식으로 넘어와
function Movie(props){
    return(
        <span>{props.title}</span>
    )
}
이런 식으로 받지만 해체할당자를 이용하여
function Movie({title}){
    return(
        <span>{title}</span>
    )
}
이렇게 깔끔하게 받을 수 있다.

slice(0, 180)
-->0번 째 부터 180까지만 보여준다.

PropTypes
--> import PropTypes from "prop-types";

Movie.propTypes(여기서 PropTypes는 에러난다) = {
  id: PropTypes.number.isRequired,
  year: PropTypes.number.isRequired,
  title: PropTypes.string.isRequired,
  summary: PropTypes.string.isRequired,
  poster: PropTypes.string.isRequired,
  genres: PropTypes.arrayOf(PropTypes.string).isRequired
};
변수들의 타입이 뭔지 모르기 때문에 지정해준다.

Link
--> import { Link } from "react-router-dom"
(npm install react-router-dom해야 import가능)
<a href="/"> 와 유사하게 <Link to=".."></Link>사용
to=".."에는 object형식으로도 가능 넘겨줄 pathname 및 state
Link를 쓰려면 Router안에서 사용해야 유효하다.

-------------------------------------------------------------

src/components/Navigation(Home,About누르면 이동(Link))
Home과 About버튼을 Link를 이용하여 생성
(당연히 Router안에 존재)

-------------------------------------------------------

src/App(최종 화면?)

Router를 사용하기위해 HashRouter, Route를 import 한다.
Route에는 중요한 props가 들어가는데 path(렌더링할 스크린 경로)로 가서 component({About})을 보여준다.

여기서 path에서 중복되는 component를 보여주지 않고 각각을 보여주기 위해서 exact={true}를 추가해야한다.


------------------------------------------------------

src/routes/Detail(Home에서 영화스티커를 누르면 상세정보를 보여주는 페이지)

const { location, history } = this.props;
--> this.props에 있는 location, history 값들을 받는다.
그런데 
if (location.state === undefined) {
      history.push("/");
    }
조건문이 있는 이유는 componentDidMount()가 render()실행 후 실행되기 때문인데 render()후 다시 실행될때 location값이 undefined가 되면 에러가 나기 때문이다.

----------------------------------------------------