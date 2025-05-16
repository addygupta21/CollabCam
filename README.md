# Welcome to CollabCam

**CollabCam** is a dynamic web platform specifically built to facilitate **online coding interviews**. Designed to streamline the interview process, CollabCam allows interviewers to engage with candidates in real-time through a collaborative environment that integrates coding, monitoring, and communication tools.

With CollabCam, interviewers can initiate a new session, share the unique session code with candidates, and dive into an interactive interview experience that mirrors the dynamics of an in-person coding test.

## Key Features

- ### **Collaborative Code Editor**
  CollabCam features a **live coding editor** where candidates can write, edit, and debug code directly in the interview session. The shared editor updates in real-time, allowing interviewers to observe the candidate’s approach, provide feedback, and guide them through technical questions or coding challenges seamlessly.

- ### **Video Monitoring**
  To ensure integrity and focus, CollabCam includes a **video monitoring tool** that tracks head movements of the candidate. When a candidate turns on their video, CollabCam will monitor and detect excessive head movements:
  - If notable movement is being detected, CollabCam issues a **warning**.
  - Continued movement post-warning may result in the **session being terminated**, allowing interviewers to maintain a controlled environment.

- ### **In-Session Chat**
  CollabCam also provides a built-in **chat feature** for smooth communication between the interviewer and candidate. Whether discussing the task at hand, providing hints, or addressing technical issues, the chat function ensures clear and effective interaction throughout the session.

> **Note**: Currently, CollabCam’s functionality is optimized for users connected on the same network due to limitations with the current PeerJS implementation. The development roadmap includes plans to expand this capability, enabling users to connect from any network globally.

## Future Scope

We have exciting plans for future updates that will enhance CollabCam’s utility:
- **Session Management**: Enable interviewers to conduct consecutive interviews without needing to reset the platform, making back-to-back interviews seamless.
- **Scoring and Feedback**: Add functionality for interviewers to assign scores, write feedback, and maintain a log of candidate performance over multiple sessions.

## Get Started

You can try out CollabCam and explore its features by visiting the link below:

[Access CollabCam](https://collabcam.onrender.com/)

---

With CollabCam, conducting remote technical interviews is simpler, more efficient, and fully interactive, providing a comprehensive environment for both interviewers and candidates.












fnikgr






2025-05-16 17:44:33,673 - INFO - PROMPT: Hello, how are you?
2025-05-16 17:47:38,977 - INFO - PROMPT: Hello, how are you?
2025-05-16 17:50:01,124 - INFO - PROMPT: 
For the project `CollabCam`:

Codebase Context:
--- File Index 0: README.md ---
# Welcome to CollabCam

**CollabCam** is a dynamic web platform specifically built to facilitate **online coding interviews**. Designed to streamline the interview process, CollabCam allows interviewers to engage with candidates in real-time through a collaborative environment that integrates coding, monitoring, and communication tools.

With CollabCam, interviewers can initiate a new session, share the unique session code with candidates, and dive into an interactive interview experience that mirrors the dynamics of an in-person coding test.

## Key Features

- ### **Collaborative Code Editor**
  CollabCam features a **live coding editor** where candidates can write, edit, and debug code directly in the interview session. The shared editor updates in real-time, allowing interviewers to observe the candidate’s approach, provide feedback, and guide them through technical questions or coding challenges seamlessly.

- ### **Video Monitoring**
  To ensure integrity and focus, CollabCam includes a **video monitoring tool** that tracks head movements of the candidate. When a candidate turns on their video, CollabCam will monitor and detect excessive head movements:
  - If notable movement is being detected, CollabCam issues a **warning**.
  - Continued movement post-warning may result in the **session being terminated**, allowing interviewers to maintain a controlled environment.

- ### **In-Session Chat**
  CollabCam also provides a built-in **chat feature** for smooth communication between the interviewer and candidate. Whether discussing the task at hand, providing hints, or addressing technical issues, the chat function ensures clear and effective interaction throughout the session.

> **Note**: Currently, CollabCam’s functionality is optimized for users connected on the same network due to limitations with the current PeerJS implementation. The development roadmap includes plans to expand this capability, enabling users to connect from any network globally.

## Future Scope

We have exciting plans for future updates that will enhance CollabCam’s utility:
- **Session Management**: Enable interviewers to conduct consecutive interviews without needing to reset the platform, making back-to-back interviews seamless.
- **Scoring and Feedback**: Add functionality for interviewers to assign scores, write feedback, and maintain a log of candidate performance over multiple sessions.

## Get Started

You can try out CollabCam and explore its features by visiting the link below:

[Access CollabCam](https://collabcam.onrender.com/)

---

With CollabCam, conducting remote technical interviews is simpler, more efficient, and fully interactive, providing a comprehensive environment for both interviewers and candidates.


--- File Index 1: src/Actions.js ---
const ACTIONS = {
    JOIN: 'join',
    JOINED: 'joined',
    DISCONNECTED: 'disconnected',
    CODE_CHANGE: 'code-change',
    SYNC_CODE: 'sync-code',
    SHARE_PEER_IDS: 'share-peer-ids',
    SIR_JOined: 'interviewer-joined',
    MONITOR: 'monitor',
    BEHAVIOUR: 'mis-behaving',
    SEND_MSG : 'send-message',
    RECV_MSG : 'receive-message',
    LANG_CHANGE: 'change-language',
    UPDATE_LAN: 'update-language',
    LEAVE: 'leave',
};

module.exports = ACTIONS;

--- File Index 2: src/index.js ---
import React from 'react';
import {createRoot} from 'react-dom/client'
import './index.css';
import App from './App';
// import { ContextProvider } from './Context';
import 'bootstrap/dist/css/bootstrap.min.css';

const root_element = document.getElementById('root');
const root= createRoot(root_element)
root.render(<App />);

// root.render(
//     <React.StrictMode>
//         {/* <ContextProvider> */}
//             <App/>
//         {/* </ContextProvider> */}
//     </React.StrictMode>
// );


--- File Index 3: src/socket.js ---
import { io } from 'socket.io-client';

export const initSocket = async () => {
    const options = {
        'force new connection': true,
        reconnectionAttempt: 'Infinity',
        timeout: 10000,
        transports: ['websocket'],
    };
    // console.log('conecting to socket');
    // return io('https://c2f-api-vineethkumarm.koyeb.app:8000', options);
    return io('https://CodeCamapi.onrender.com', options);
    // return io('https://amazing-croissant-5bbe89.netlify.app/', options);
    // return io('https://c2f-api.el.r.appspot.com', options);
    // return io('http://localhost:3007',options);
    // return io()
    console.log(process.env);
    return io(process.env.REACT_APP_BACKEND_URI, options);
};

--- File Index 4: src/App.js ---
import './App.css';
import React,{createContext, useReducer} from 'react';
import {Route , BrowserRouter as Router,Routes} from 'react-router-dom'
import { initialState, reducer } from './reducers/userReducer';
import 'react-bootstrap'
//components and pages
import MyNavbar from './components/navbar';
import Home from './pages/home';
import NotFound from './pages/NotFound';
// import Testing from './pages/testing';
import { Toaster } from 'react-hot-toast';
import Slides from './pages/slides';
import AddSlides from './pages/addSlides';
import PeerCall from './pages/peerCall';
import Motivation from './pages/motivation';
import Features from './pages/features';
import Footer from './components/footer';

export const UserContext = createContext();

function App() {


	const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <UserContext.Provider value={{state,dispatch}}>
            <div>
                <Toaster 
                    position='top-right'
                />
            </div>
            <Router >
                <div className='wrapper' >

                <div className='App'>
                    <MyNavbar/>
                    <div className='content d-flex'>
                        <Routes>
                                <Route eaxct path="/" element={<Home/>} />
                                {/* <Route exact path="/call/:roomId" element={<CallPage/>} /> */}
                                <Route exact path="/call/:roomId" element={<PeerCall/>} />
                                <Route exact path='/slides' element={<Slides/>} />
                                <Route exact path='/add' element={<AddSlides/>} />
                                {/* <Route path="/chat" element={<Chat/>} /> */}
                                <Route exact path='/motivation' element={<Motivation />} />
                                {/* <Route exact path='/features' element={<Features />} /> */}
                                <Route path="*" element={<NotFound/>} />
                        </Routes>
                    </div>
                </div>
                    {/* <Footer /> */}
                </div>
            </Router>
         </UserContext.Provider>
		
	
	);
}

export default App;


--- File Index 5: src/components/navbar.js ---
import React,{ useContext } from "react";
import { Link } from "react-router-dom";
import { UserContext } from "../App";
import { useNavigate } from "react-router-dom";
import { NavbarBrand } from "react-bootstrap";
import "../styles/navbar.css"

const Navbar = () => {
	let { state, dispatch } = useContext(UserContext);
	const history = useNavigate()
	let jwt = localStorage.getItem("jwt")
	if(jwt) {
		state=true;
	}


	const renderList = () => {
		if (state) {
			return [
				<>
					<Link key='profile' className="navlink" to="/profile">Profile</Link>
					<Link key='dsf' className="navlink" to="/newPost">new post</Link>
					<button key={jwt.slice(0,3)} className="btn btn-primary" onClick={() => {
							localStorage.clear();
							dispatch({type:"CLEAR"})
							history('/')
						}
						}>
						Logout
					</button>
				
				</>
			];
		} else {
			return [
				<>
                    {/* <Link to='/testing' className="navlink">Test</Link>  */}
					{/* <Link key='login' className="navlink" to="/login">Login</Link> */}
					{/* <Link key='register' className="navlink" to="/register">Register</Link> */}
					<Link key='slides' className="navlink" to="/motivation">Motivation</Link>
					{/* <Link key='slides' className="navlink" to="/features">Features</Link> */}
					<Link key='slides' className="navlink" to="/slides">Revision</Link>
					
				</>,
			];
		}
	};

	return (
		<nav className="main-nav">
			{/* <div className="logo" ><h3 href="/">CodeCam</h3></div> */}
            <NavbarBrand className="logo">
                <Link key='logo' to="/">
                    CodeCam
                    {/* Test */}
                </Link>
            </NavbarBrand>
			<div className="navlinks">{renderList()}</div>
		</nav>
	);
};

export default Navbar;

--- File Index 6: src/components/peditor.js ---
import React, { useEffect, useState, useRef, useMemo } from 'react'
import { useLocation } from 'react-router-dom';
import { Button, Form } from 'react-bootstrap';
// codemirror components
import { useCodeMirror } from '@uiw/react-codemirror';
import Select from 'react-select';

// import languages 
import { javascript } from '@codemirror/lang-javascript';
import { cpp } from '@codemirror/lang-cpp';
import { python } from '@codemirror/lang-python';
import { java } from '@codemirror/lang-java';

// import themes 
import { githubDark } from '@uiw/codemirror-theme-github';
import { xcodeDark, xcodeLight } from '@uiw/codemirror-theme-xcode';
import { eclipse } from '@uiw/codemirror-theme-eclipse';
import { abcdef } from '@uiw/codemirror-theme-abcdef';
import { solarizedDark } from '@uiw/codemirror-theme-solarized';
import "../styles/editor.css"
import { toast } from 'react-hot-toast';

var qs = require('qs');


// const extensions = [javascript()]


const Editor = ({ sendHandler, roomId, onCodeChange, code, lang }) => {
    const history = useLocation()
    const location = useLocation()
    const uname = location?.state?.username
    const [theme, setTheme] = useState(githubDark);
    // const [code, se==1ode] = useState(code);
    const [selectValue, setSelectValue] = useState('javascript')
    const [extensions, setExtensions] = useState([javascript()])
    const [placeholder, setPlaceholder] = useState('Please enter the code.');
    const [input, setInput] = useState('')
    const [output, setOutput] = useState('')
    const [ran, setran] = useState(false)
    const [tc, setTc] = useState(true)
    const thememap = new Map()
    const langnMap = new Map()

    const editorRef = useRef(null)
    const { setContainer } = useCodeMirror({
        container: editorRef.current,
        extensions,
        value: code,
        theme,
        editable: true,
        height: `70vh`,
        width: `45vw`,
        basicSetup: {
            foldGutter: false,
            dropCursor: false,
            indentOnInput: false,
        },
        options: {
            autoComplete: true,
        },
        placeholder: placeholder,
        style: {
            position: `relative`,
            zIndex: `999`,
            borderRadius: `10px`,
        },
        onChange: (value) => {
            onCodeChange(value)
            sendHandler(3, { value })
        }

    })


    const themeInit = () => {
        thememap.set("githubDark", githubDark)
        thememap.set("xcodeDark", xcodeDark)
        thememap.set("eclipse", eclipse)
        thememap.set("xcodeLight", xcodeLight)
        thememap.set("abcdef", abcdef)
        thememap.set("solarizedDark", solarizedDark)
    }

    const langInit = () => {
        langnMap.set('java', java)
        langnMap.set('cpp', cpp)
        langnMap.set("javascript", javascript)
        langnMap.set('python', python)
    }


    const handleThemeChange = (event) => {
        setTheme(thememap.get(event.value));
    };

    const handleLanguageChange = (event) => {
        setExtensions([langnMap.get(event.value)()]);

        sendHandler(4, { newLang: event.value })
        setSelectValue(event.value)
    };

    themeInit()
    langInit()

    function langCode(e) {
        if (e == 'javascript') return 'js';
        else if (e == 'python') return 'py';
        return e;
    }

    useEffect(() => {
        if (!editorRef.current) {
            alert('error loading editor')
            history('/')
        }
        if (editorRef.current) {
            setContainer(editorRef.current)
        }

    }, [editorRef.current])

    useEffect(() => {
        if (lang != selectValue && lang) {
            setSelectValue(lang)
            setExtensions([langnMap.get(lang)()]);
        }
    }, [lang])

    const compile = (e) => {
        e.preventDefault();
        var data = qs.stringify({
            'code': code,
            'language': langCode(selectValue),
            'input': input
        });
        var config = {
            method: 'post',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            },
            body: data
        };
        fetch('https://api.codex.jaagrav.in', config)
            .then(res => res.json())
            .then(data => {
                if (data['error'].length == 0) {
                    setTc(true)
                    toast.success("compiled sucessfully")
                    setOutput(data['output'])
                }
                else {
                    setTc(false)
                    toast.error("compilation error")
                    setOutput(data['error'])
                }
            })
            .catch(function (error) {
                console.log(error);
            });
    }

    const handleInputChange = (e) => {
        setInput(e.target.value)
    }
    const handleOutputChange = (e) => {
        setOutput(e.target.value)
    }

    const themeOptions = [
        { value: "githubDark", label: "Github Dark" },
        { value: "eclipse", label: "Eclipse" },
        { value: "xcodelight", label: "X Code Light" },
        { value: "xcodedark", label: "X Code Dark" },
        { value: "solarizeddark", label: "Solarized Dark" },
        { value: "abcdef", label: "ABCDEFs" },
    ]

    const langOptions = [
        { value: "javascript", label: "JavaScript" },
        { value: "java", label: "Java" },
        { value: "cpp", label: "C++" },
        { value: "python", label: "Python" }
    ]

    return (
        <div className='editorcomponent'>
            <div style={{
                display: "flex",
                gap: "10px",
                flexDirection: "column"
            }}>
                <div>
                    <span>Theme:</span>
                    <Select
                        onChange={handleThemeChange}
                        classNamePrefix="select"
                        defaultValue={themeOptions[0]}
                        name="color"
                        options={themeOptions}
                    />
                </div>
                <div>
                    <span>Language:</span>
                    <Select
                        onChange={handleLanguageChange}
                        className="basic-single"
                        classNamePrefix="select"
                        defaultValue={langOptions[0]}
                        name="color"
                        options={langOptions}
                    />
                </div>
                <button className='run ' onClick={compile} >Run</button>
            </div>
            <div style={{marginTop: "20px"}} ref={editorRef} className='ide' ></div>
            <div style={{display: "flex", flexDirection: "row", justifyContent: "space-between", marginTop: "20px"}} className='iodiv d-flex flex-wrap'>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Input</Form.Label>
                    <Form.Control as="textarea" rows={2} value={input} onChange={handleInputChange} />
                </Form.Group>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Output</Form.Label>
                    <Form.Control as="textarea" rows={2} value={output} className={tc ? 'ctxt' : 'etxt'} onChange={handleOutputChange} disabled />
                </Form.Group>

            </div>
        </div>
    );
}

export default Editor

--- File Index 7: src/components/footer.js ---
import React from 'react'
import "../styles/footer.css"
const Footer = () => {
  return (
    <footer className="page-footer font-small bg-dark text-center text-white pt-2">

        <div className="footer-copyright text-center py-3">
            <h5>Made With ❤️ by Aaditya Kumar </h5>
        </div>

    </footer>
    )
}

export default Footer


--- File Index 8: src/components/pchat.js ---
import React, {useEffect, useState} from 'react'
import { Form } from 'react-bootstrap'

import "../styles/chat.css"

const Chat = ({sendHandler, roomId, username}) => {

    const [msg, setmsg] = useState('')

    const handleChange = (e) => {
        setmsg(e.target.value)
    }
    const handleSend = (e) => {
        e.preventDefault();
        if(msg.length==0) return;
        sendHandler(2,{username, msg})
        addSendMsg(msg)
        setmsg('')
    }
    
    const addSendMsg = (text) => {
        const element = 
            ` <div class='send'>
                    <div class='msg'>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML=element
        rdiv.setAttribute('class', 'msg-container')
        
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)  
        par.scrollTop = par.scrollHeight  
    }


    return (
        <div className='chatCont mt5'>
            <div className='chatBox'>
                <div className='scroller' id='msg-div'>
                </div>
                <div className='minp'>
                    <Form onSubmit={handleSend}>

                    
                        <Form.Control
                            type="text" 
                            placeholder="Enter message" 
                            name="msg"
                            onChange={handleChange}
                            value={msg}
                            className='finp inlin'
                        />
                        {/* <Button className='inlin' onClick={handleSend}> Send</Button> */}
                    </Form>
                </div>
            </div>
        </div>
    )
}

export default Chat

--- File Index 9: src/pages/NotFound.js ---
import React from 'react'

const NotFound = () => {
  return (
    <div className='errorpage'><span>Sorry, the request resources are not available on this website</span></div>
  )
}

export default NotFound

--- File Index 10: src/pages/features.js ---
import React from 'react'

const Features = () => {
  return (
    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Features</h2>
        <ul>
            <li>Video Chatting</li>
            <li>Screen Sharing</li>
            <li>Face Monitoring</li>
            <li>Collaborative Code Editor</li>
            <li>Online Compiler</li>
        </ul>
    </div>
  )
}

export default Features

--- File Index 11: src/pages/motivation.js ---
import React from 'react'

const Motivation = () => {
  return (

    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Motivation</h2>
        <p>Introducing the ultimate solution for seamless and efficient online interviews - this innovative 
            project brings together the power of video chat, code editor, and compiler all in one platform!
             No more juggling between multiple tabs or applications during interviews. With this website, you 
             can now focus solely on showcasing your skills and acing those coding challenges. Embrace the 
             ease of navigating through the interview process effortlessly, while impressing your potential 
             employers with your coding prowess.

        </p>
    </div>
  )
}

export default Motivation

--- File Index 12: src/pages/home.js ---
import React, { useState } from 'react'
import {  Form, Button } from 'react-bootstrap'
import {useNavigate} from 'react-router-dom'
import ShortUniqueId from 'short-unique-id';
import toast from 'react-hot-toast'
import copy from 'copy-to-clipboard';
import { Carousel } from 'react-bootstrap';
import {InfoCircleFill} from 'react-bootstrap-icons'
import Footer from '../components/footer';

const Home = () => {
    const Suid = new ShortUniqueId()
    const history = useNavigate()
    const valt = 'Enter the Session Id', invalt = 'Invalid Session  Id';
    const lenvalt = 'Enter only 10 characters Session Id';
    const [uname, setuname] = useState('')    
    const [fclick, setFclick] = useState(false)
    const [sid, setSid] = useState('')
    const [sid1, setSid1] = useState('')
    const [validText, setvalidText] = useState(valt)
    const alphanumericRegex = /^[a-zA-Z0-9]+$/;
    const [interviewer, setInterviewer] = useState(false)
  

    const handleChange1 = (e) => {
        const input = e.target.value
        if (input && !alphanumericRegex.test(input)) {
            setvalidText(invalt)
            setSid1('')
            return;
        } 
        setvalidText(lenvalt)
        if(input.length>10) {
            setSid1('')
            return
        }
        setSid1(input)
    }

    const handleNameChange = (e) => {
        setuname(e.target.value)
    }

    const generateCode = (e) => {
        e.preventDefault();
        setFclick(true)
        let code  = Suid(10);
        setSid(code)
        setSid1(code)
        if (copy(code))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    const handleSwitch = (e) => {
        setInterviewer(!interviewer)
        // console.log(interviewer);
    }

    const handleJoin = async (e)=> {
        e.preventDefault()
        if(sid.length===0 && sid1.length===0) {
            toast('Please generate or fill the Session ID', {
                icon: '❕',
              });
            return;
        }
        if(uname.length==0) {
            toast.error("Name field is empty")
            return;
        }
        const roomId = sid.length>0?sid:sid1
        if(sid.length>0) {
            localStorage.setItem('init', 'true')
        }
        toast.success('Joining new Call')
        history(`/call/${roomId}`, {
            state: {
                username: uname,
                interviewer:!interviewer,
            },
        });
    }
    
    return (
        <>
        <div className='main-home'>
            <div className='cen-container'>
                {/* <Image src='/qna.png' className='img-fluid' /> */}
                <div className='lcentral l-shadow'>
                <Carousel data-bs-theme="dark" className='fwid'>
                    <Carousel.Item key='1' interval={500}>
                        <img
                        className="d-block w-100 img-fluid"
                        src="/qna.png"
                        alt="First slide"
                        />
                        <Carousel.Caption>
                        <h5 className='blk-txt'>Online Interviews</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='2' interval={750}>
                        <img alt='face motion' className="d-block w-100 img-fluid" src='/face scanner.png'/>
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Face Motion Detection</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='3' interval={750}>
                        <img alt='video' className="d-block w-100 img-fluid"  src="/interview.jpg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Video Interview</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='4' interval={750}>
                        <img alt='code editor' className="d-block w-100 img-fluid"  src="/editor.jpeg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Live Code Editor</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='5' interval={750}>
                        <img alt='live chat' className="d-block w-100 img-fluid"  src="/chat.png" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'> Live Chat</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    
                </Carousel>
                </div>
            </div>
            <div className='cen-container'>
                <div className='central r-shadow'>
                    <div className='d-flex flex-column'>
                        <div className='jcontent'>
                            <h3 className='t-color'>Welcome to <span className='myfont'>CodeCam</span></h3>

                        </div>
                        <div className='jcontent mt4'>
                            <h4>Start a New Session</h4>
                            {/* <Form> */}
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    
                                    <Form.Control 
                                        className='genInp' 
                                        type="text" 
                                        placeholder="#uniqueId" 
                                        disabled 
                                        name="sid"
                                        value={sid}
                                    />
                                        <Button className="btn-dark gen-btn"
                                            onClick={generateCode}
                                        >Generate {fclick ? 'Again' : ''}</Button>
                                </Form.Group>
                                <h4>or</h4>
                            
                        </div>
                        <div className='jcontent mt-4'>
                            <h4>Join with Code</h4>
                                <Form.Group className="mmb fin " >
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Id" 
                                        name="sid1"
                                        onChange={handleChange1}
                                        value={sid1}
                                        
                                    />
                                    <Form.Text>{validText}</Form.Text>
                                </Form.Group>
                        </div>
                        <div className='jcontent'>
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Enter your good Name" 
                       
                                        name="uname"
                                        onChange={handleNameChange}
                                        value={uname}
                                    />
                                </Form.Group>
                                <Form.Group className='ml-2'>
                                    <Form.Check
                                        type="switch"
                                        id="custom-switch"
                                        label="Join as Interviewer "
                                        value={interviewer}
                                        name='interviewer'
                                        onChange={handleSwitch}
                                        style={{display: 'inline', textAlign: 'left !important', padding: '5px'}}
                                    />
                                    <InfoCircleFill className='infoIcon'></InfoCircleFill>
                                    <span className="afterHoverText">Toggle to disable Face Monitoring</span>
                                </Form.Group>
                                <Form.Group className='mt-2'>
                                    <Button onClick={handleJoin}>Join Call</Button>
                                </Form.Group>

                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div className='disclaimer'>
            <h5>Note:</h5>
            <p>Sit in well lit room and Face camera to avoid misbehaviour notifications. <br></br>
            If your video is not visible, hard reload the page and rejoin.
            </p>
        </div>
        </>
    )
}

export default Home

--- File Index 13: src/pages/peerCall.js ---
import React, { useState, createContext, useRef, useEffect } from "react";

import toast from 'react-hot-toast';
import { useLocation, useNavigate, useParams, Navigate } from 'react-router-dom';
import { Peer } from 'peerjs'
import { Button } from "react-bootstrap";
import * as faceapi from 'face-api.js'
import Chat from "../components/pchat";
import copy from 'copy-to-clipboard';
import Editor from "../components/peditor";
import "../styles/callpage.css";
const server = process.env.REACT_APP_BACKEND_URI;
const server_host = process.env.REACT_APP_BACKEND_HOST;
const PeerCall = () => {
    const location = useLocation();
    const history = useNavigate();
    const { roomId } = useParams();

    const myName = location.state?.username
    const interviewer = location.state?.interviewer

    const MOTION_THRESHOLD = 40;
    // const myConns = new Map()
    const [myConns, setMyConns] = useState(new Map())
    const [mycalls, setMyCalls] = useState(new Set())
    const idName = new Map()


    var userMoves = 0;
    var previousLandmarks = null;
    var warned = false;
    var peer = null
    var dataStream = null
    var screenStream = null
    var myPeerId;
    var timer;

    const [allPeers, setAllPeers] = useState([])
    const [stream, setstream] = useState(null)

    const [lang, setLang] = useState('javascript')
    const [chatState, setChatState] = useState(false)
    const [displayCheck, setDisplayCheck] = useState(false)
    const [sirId, setsirId] = useState(null)
    const myVideo = useRef()
    const [code, setcode] = useState(`console.log("Live shared Code Editor")`)

    //Face motion logic
    function analyzeFaceMotions(landmarks) {
        const currentLandmarks = landmarks._positions;

        if (previousLandmarks && currentLandmarks.length == 68) {
            let totalMotion = 0, averageMotion = 0;
            for (let i = 0; i < 68; i++) {
                const dx = currentLandmarks[i]._x - previousLandmarks[i]._x;
                const dy = currentLandmarks[i]._y - previousLandmarks[i]._y;
                const distance = Math.sqrt(dx * dx + dy * dy)
                totalMotion += distance
            }
            averageMotion = totalMotion / 68;
            // console.log(averageMotion);
            if (averageMotion > MOTION_THRESHOLD) {
                toast('Face motion detected, Please concentrate on the interview', {
                    icon: '❕',
                });
                userMoves++;
            }
        }
        previousLandmarks = currentLandmarks;
    }

    async function detectFaceMotions() {
        if (!myVideo.current) return;

        timer = setInterval(async () => {
            if (warned) {
                if (sirId) { }
                // socketRef?.current?.emit(ACTIONS.BEHAVIOUR , {roomId})
                else {
                    toast.error('Please Behave until the Interviewer joins')
                }
                warned = false
                userMoves = 0
                return
            }
            if (userMoves > 12) {
                toast.error("Warning! Interviewer will be notified if movement is observed again.")
                userMoves = userMoves - 2;
                warned = true;
            }
            const detections = await faceapi.detectAllFaces(myVideo.current,
                new faceapi.TinyFaceDetectorOptions())
                .withFaceLandmarks()

            if (myVideo.current && detections.length == 0) {
                toast.error("Please sit in a well lit room and face the Webcam!")
                alert('Face cannot be detected ! Please sit in a weel lit area.')
                userMoves++;
            }
            else if (detections?.length > 1) {
                toast.error(`${detections.length} persons spotted in camera`)
                userMoves++;
            }
            else if (detections && detections.length == 1) {
                analyzeFaceMotions(detections[0].landmarks)
            } else {
                console.log(detections);
                toast.error(" Please face the webcam!")
            }

        }, 2500)
    }

    useEffect(() => {
        //take camera permission
        navigator?.mediaDevices?.getUserMedia({ video: true, audio: true })
            .then(videoStream => {
                dataStream = videoStream;
                setstream(videoStream);

                // initialize socket
                init(videoStream)

            })
            .catch((error) => {
                console.log(error);
                toast.error('Cannot connect without video and audio permissions')
                return <Navigate to="/" />;
            });
        //load faceapi Models
        loadModels()
        //socket connecting function
        const init = async (videoStream) => {
            // PeerJS functionality starts 
            var peer_params = {
                host: server_host,
                path: '/myapp',
            }
            if (server_host == 'localhost') peer_params['port'] = 3007
            peer = new Peer(peer_params)
            // once peer created join room
            peer.on('open', function (id) {
                myPeerId = id

                addVideo(videoStream, id, myName)
                if (interviewer)
                    detectFaceMotions()
                let flag = interviewer ? false : true
                fetch(`${server}join`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        username: myName,
                        peerId: id,
                        flag
                    })
                }).then(res => res.json())
                    .then(res_arr => {
                        const { data } = res_arr
                        setAllPeers(data);
                        for (const { peerId, username } of data) {

                            let conn = peer.connect(peerId)

                            conn.on("open", () => {
                                myConns.set(conn, username)
                                conn.send({ sig: 1, data: { id, name: myName } })
                                let call = peer.call(peerId, videoStream)
                                call.on('stream', (remoteStream) => {
                                    mycalls.add(call)
                                    addVideo(remoteStream, peerId, username)
                                })
                            })

                            conn.on('data', ({ sig, data }) => {
                                recvHandler(conn, sig, data)
                            })

                            conn.on('close', () => {
                                myConns.delete(conn)
                                const element = document.getElementById(peerId);
                                element?.remove();
                            })

                            if (peerId == sirId) {
                                toast.success('Interviewer Joined')
                            }
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    })

            });

            peer.on("connection", (conn) => {
                conn.on('data', ({ sig, data }) => {
                    recvHandler(conn, sig, data)
                })
                conn.on('close', () => {
                    myConns.delete(conn)
                    toast.success(`${idName[conn.peer]} left the Call`)
                    const element = document.getElementById(conn.peer);
                    element?.remove();
                })
            });

            peer.on('call', (call) => {
                call.answer(videoStream)
                mycalls.add(call)
                call.on('stream', (remoteStream) => {
                    addVideo(remoteStream, call.peer, idName[call.peer])
                })
                toast.success(`${idName[call.peer]} joined the Call`)
            })
        };

        // Clean up function to remove camera permissions and end socket
        return () => {
            const ele = document.getElementById(myPeerId)
            ele?.remove()
            clearInterval(timer)
            if (peer) {
                try {
                    fetch(`${server}leave`, {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json"
                        },
                        body: JSON.stringify({
                            roomId,
                            peerId: myPeerId,
                        })
                    })
                } catch (err) {
                    console.log(err);
                    toast.error("Coudn't leave the room at the current moment")
                }
                peer.destroy();
            }
            if (dataStream) {
                const tracks = dataStream.getTracks();
                tracks.forEach((track) => track.stop());
            }
        };
    }, []);

    //Funtions to be triggered by child components passed as props

    const sendHandler = (sig, data) => {
        for (const [conn, name] of myConns) {
            conn.send({ sig, data })
        }
    }

    const recvHandler = (conn, sig, data) => {
        switch (sig) {
            case 1: {
                //share name
                const { id, name } = data
                myConns.set(conn, name)
                idName[id] = name
                conn.send(3, { value: code })
                break;
            }
            case 2: {
                // chat msg
                const { username, msg } = data
                addRecvMsg(msg, username)
            }
            case 3: {
                // code update
                const { value } = data
                console.log(value);
                if (value != code)
                    setcode(value);
            }
            case 4: {
                // language change
                const { newLang } = data
                if (lang != newLang) setLang(newLang)
            }
            default:
                break;
        }
    }

    const addRecvMsg = (text, name = 'Unknown') => {
        setChatState(true)
        const element =
            ` <div class='receive'>
                    <div class='msg'>
                    <span class='senderName' >${name}</span>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML = element
        rdiv.setAttribute('class', 'msg-container')
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)
        if (par)
            par.scrollTop = par.scrollHeight
    }


    //Functions related to this component
    function addVideo(vstream, peerID, userName = "user") {
        const prev = document.getElementById(peerID)
        prev?.remove()
        const row = document.createElement('div')
        row.setAttribute('class', 'mt-2')
        row.setAttribute('id', peerID)

        const video = document.createElement('video')
        video.srcObject = vstream;
        video.addEventListener('loadedmetadata', () => {
            video.play()
        })
        // console.log(peerID, myPeerId);
        if (peerID == myPeerId) {
            video.muted = true
            myVideo.current = video
        }

        const span = document.createElement('span')
        span.innerText = userName
        span.setAttribute('class', 'tagName')

        row.append(video)
        row.append(span)

        const exist = document.getElementById(peerID)
        if (exist) return

        const peerDiv = document.getElementById('peerDiv')

        peerDiv?.insertBefore(row, peerDiv.children[0])

    }

    const loadModels = () => {
        Promise.all([
            faceapi.nets.faceLandmark68Net.loadFromUri("/models"),
            faceapi.nets.tinyFaceDetector.loadFromUri("/models"),
            faceapi.loadFaceLandmarkTinyModel("/models")
        ]).then(() => {
            // console.log('Models Loaded');
        }).catch(err => {
            console.log('FaceAPI modules loading error', err);
        })

    }

    const screenShareHandler = async (e) => {
        e.preventDefault()
        setDisplayCheck(!displayCheck)
        idName['screen'] = 'screen'
        navigator?.mediaDevices?.getDisplayMedia({ audio: true }).then(displStream => {
            screenStream = displStream
            addVideo(displStream, 'screen', 'Screen')
            replaceStream(displStream)
        }).catch((error) => {
            console.log(error);
        });

    }

    const replaceStream = (mediaStream) => {
        for (const call of mycalls) {
            for (let sender of call.peerConnection?.getSenders()) {
                if (sender.track.kind == "audio") {
                    if (mediaStream.getAudioTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getAudioTracks()[0]);
                    }
                }
                if (sender.track.kind == "video") {
                    if (mediaStream.getVideoTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getVideoTracks()[0]);
                    }
                }
            }
        }
    }


    function stopCapture(evt) {
        evt.preventDefault()
        setDisplayCheck(!displayCheck)
        replaceStream(stream)
        let ele = document.getElementById('screen')
        const evid = ele?.childNodes[1];
        if (evid && evid.srcObject) {
            const tracks = evid.srcObject.getTracks();
            tracks.forEach((track) => track.stop());
        }
        ele?.remove();
    }
    const copyCode = (e) => {
        e.preventDefault();
        if (copy(roomId))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    function chatHider() {
        setChatState(chatState => !chatState)
    }

    function leaveRoom() {
        if (peer) {
            try {
                fetch(`${server}leave`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        peerId: myPeerId,
                    })
                })
            } catch (err) {
                console.log(err);
                toast.error("Coudn't leave the room at the current moment")
            }
            peer.destroy()
        }
        if (dataStream) {
            const tracks = dataStream.getTracks();
            tracks.forEach((track) => track.stop());
        }
        clearInterval(timer)
        history('/');
    }

    if (!location.state) {
        return <Navigate to="/" />;
    }


    return (
        <>
            <div style={{ marginBottom: "50px", display: "flex", flexDirection: "row", justifyContent: "space-between" }} >
                <div style={{ marginTop: "20px" }}>
                    <div className='econt'>
                        <Editor
                            conns={myConns}
                            roomId={roomId}
                            onCodeChange={setcode}
                            code={code}
                            lang={lang}
                            peer={peer}
                            sendHandler={sendHandler}
                        />
                    </div>
                </div>
                <div style={{
                    display: "flex",
                    flexDirection: "column",
                    alignItems: "center",
                    width: "100%",
                    marginTop: "20px"
                }}>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        gap: "20px",
                        width: "100%",
                    }}>
                        <Button
                            onClick={leaveRoom}
                            className="mt-2 btn-danger"
                            style={{ width: '120px', margin: 'auto' }}>Leave</Button>
                        <Button
                            onClick={!displayCheck ? screenShareHandler : stopCapture} className="mt-2" style={{ width: '170px', margin: 'auto' }}>{!displayCheck ? 'Screen Share' : 'Stop Share'}</Button>
                        <Button
                            onClick={copyCode}
                            className="mt-2 btn-info"
                            style={{ width: '160px', margin: 'auto' }}>
                            Copy Session Id
                        </Button>
                        <div onClick={chatHider}>
                            {!chatState ?
                                <img style={{ width: "50px" }} src="/toggle_chat.png"></img>
                                : <></>
                            }
                        </div>
                    </div>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        marginTop: "100px",
                        width: "100%",
                        justifyContent: "space-evenly"
                    }} id="peerDiv">
                        {/* <div style={{ margin: "0px 10px" }} className='' id="users">
                        </div> */}
                    </div>
                </div>

            </div >

            {!chatState ?
                <></>
                : <>
                    <Chat
                        recvMsg={addRecvMsg}
                        conns={myConns}
                        roomId={roomId}
                        username={myName}
                        sendHandler={sendHandler}
                        peer={peer}
                        className='chatCont'
                    />
                    <img src="/close_chat.png" onClick={chatHider} className="fix_close"></img>
                </>
            }
        </>
    )
}

export default PeerCall

--- File Index 14: src/pages/addSlides.js ---
import React, {useState} from 'react'
import { Form , Button} from 'react-bootstrap'
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const AddSlides = () => {

    const [heading, setHeading] = useState("");
    const [body, setBody] = useState("");

    const handleSubmit = async (e) => {
        e.preventDefault();

        const data = {
        heading: heading,
        body: body
        };

        try {
            const response = await fetch(`${serverLink}/save_data`, {
                method: "POST",
                headers: {
                "Content-Type": "application/json"
                },
                body: JSON.stringify(data)
            });
          
            if (response.ok) {
            //   console.log("Data saved successfully");
                toast.success("Slides Updated")
            } else {
              console.error("Error saving data:", response.status);
              toast.error('Error updating')
            }
          } catch (error) {
            console.error("Error saving data:", error);
            toast.error("Error sending data")
          }
          

        // Reset form fields
        setHeading("");
        setBody("");
    };



    return (
        <div className='cen-container mt-5 lcentral l-shadow'>
            <h3>Add New Slides</h3>
            <form onSubmit={handleSubmit} >
                {/* <label htmlFor="heading">Heading:</label> */}
                {/* <input
                    type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required
                /> */}
                
                {/* <label htmlFor="body">Body:</label>
                <textarea
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                ></textarea> */}
                <Form.Group className="mb-3" >
                    <Form.Label>Heading:</Form.Label>
                    <Form.Control type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required />
                </Form.Group>
                <Form.Group className="mb-3" >
                    <Form.Label>Body:</Form.Label>
                    <Form.Control as="textarea" 
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                    rows={3} />
                </Form.Group>

                <Button type="submit">Submit</Button>
            </form>
        </div>
    )
}

export default AddSlides

--- File Index 15: src/pages/slides.js ---
import React, {useState, useEffect} from 'react'
import { Carousel } from 'react-bootstrap';
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const Slides = () => {
    let local_Data = localStorage.getItem('slides')
    const [data, setData] = useState(local_Data!=undefined ? JSON.parse(local_Data) : [])

    useEffect(() => {
        async function fetchData() {
            try {
                const res= await fetch(`${serverLink}get_data`, {
                    method: "GET",
                    headers: {
                    "Content-Type": "application/json"
                    },
                });
                const newData = await res.json();
                setData(newData)
                localStorage.setItem('slides', JSON.stringify(newData))
            } catch(error) {
                console.log(error);
                toast.error("Error receiving data")
            }
        }
        fetchData();
        
    }, [])

    return (
        <div className=''>
            <div className='cen-container l-shadow lcentral mt-5 '>
                <Carousel>
                {   
                    data.map((item, index) => (
                        
                        <Carousel.Item key={index} interval={500}>
                            <div key={index} className='f-height'>
                                <h2>{item.heading}</h2>
                                <h6>{item.body }</h6>
                            </div>
                        </Carousel.Item>

                ))}
                </Carousel>
            </div>
        </div>
    )
}

export default Slides

--- File Index 16: src/reducers/userReducer.js ---
export const initialState = null;

export const reducer = (state, action) => {
	if (action.type === "USER") {
		return action.payload;
	}if (action.type === "CLEAR") {
		return null;
	}
	return state;
};



Analyze the codebase context.
Identify the top 5-10 core most important abstractions to help those new to the codebase.

For each abstraction, provide:
1. A concise `name`.
2. A beginner-friendly `description` explaining what it is with a simple analogy, in around 100 words.
3. A list of relevant `file_indices` (integers) using the format `idx # path/comment`.

List of file indices and paths present in the context:
- 0 # README.md
- 1 # src/Actions.js
- 2 # src/index.js
- 3 # src/socket.js
- 4 # src/App.js
- 5 # src/components/navbar.js
- 6 # src/components/peditor.js
- 7 # src/components/footer.js
- 8 # src/components/pchat.js
- 9 # src/pages/NotFound.js
- 10 # src/pages/features.js
- 11 # src/pages/motivation.js
- 12 # src/pages/home.js
- 13 # src/pages/peerCall.js
- 14 # src/pages/addSlides.js
- 15 # src/pages/slides.js
- 16 # src/reducers/userReducer.js

Format the output as a YAML list of dictionaries:

```yaml
- name: |
    Query Processing
  description: |
    Explains what the abstraction does.
    It's like a central dispatcher routing requests.
  file_indices:
    - 0 # path/to/file1.py
    - 3 # path/to/related.py
- name: |
    Query Optimization
  description: |
    Another core concept, similar to a blueprint for objects.
  file_indices:
    - 5 # path/to/another.js
# ... up to 10 abstractions
```
2025-05-16 17:50:23,136 - INFO - PROMPT: 
For the project `CollabCam`:

Codebase Context:
--- File Index 0: README.md ---
# Welcome to CollabCam

**CollabCam** is a dynamic web platform specifically built to facilitate **online coding interviews**. Designed to streamline the interview process, CollabCam allows interviewers to engage with candidates in real-time through a collaborative environment that integrates coding, monitoring, and communication tools.

With CollabCam, interviewers can initiate a new session, share the unique session code with candidates, and dive into an interactive interview experience that mirrors the dynamics of an in-person coding test.

## Key Features

- ### **Collaborative Code Editor**
  CollabCam features a **live coding editor** where candidates can write, edit, and debug code directly in the interview session. The shared editor updates in real-time, allowing interviewers to observe the candidate’s approach, provide feedback, and guide them through technical questions or coding challenges seamlessly.

- ### **Video Monitoring**
  To ensure integrity and focus, CollabCam includes a **video monitoring tool** that tracks head movements of the candidate. When a candidate turns on their video, CollabCam will monitor and detect excessive head movements:
  - If notable movement is being detected, CollabCam issues a **warning**.
  - Continued movement post-warning may result in the **session being terminated**, allowing interviewers to maintain a controlled environment.

- ### **In-Session Chat**
  CollabCam also provides a built-in **chat feature** for smooth communication between the interviewer and candidate. Whether discussing the task at hand, providing hints, or addressing technical issues, the chat function ensures clear and effective interaction throughout the session.

> **Note**: Currently, CollabCam’s functionality is optimized for users connected on the same network due to limitations with the current PeerJS implementation. The development roadmap includes plans to expand this capability, enabling users to connect from any network globally.

## Future Scope

We have exciting plans for future updates that will enhance CollabCam’s utility:
- **Session Management**: Enable interviewers to conduct consecutive interviews without needing to reset the platform, making back-to-back interviews seamless.
- **Scoring and Feedback**: Add functionality for interviewers to assign scores, write feedback, and maintain a log of candidate performance over multiple sessions.

## Get Started

You can try out CollabCam and explore its features by visiting the link below:

[Access CollabCam](https://collabcam.onrender.com/)

---

With CollabCam, conducting remote technical interviews is simpler, more efficient, and fully interactive, providing a comprehensive environment for both interviewers and candidates.


--- File Index 1: src/Actions.js ---
const ACTIONS = {
    JOIN: 'join',
    JOINED: 'joined',
    DISCONNECTED: 'disconnected',
    CODE_CHANGE: 'code-change',
    SYNC_CODE: 'sync-code',
    SHARE_PEER_IDS: 'share-peer-ids',
    SIR_JOined: 'interviewer-joined',
    MONITOR: 'monitor',
    BEHAVIOUR: 'mis-behaving',
    SEND_MSG : 'send-message',
    RECV_MSG : 'receive-message',
    LANG_CHANGE: 'change-language',
    UPDATE_LAN: 'update-language',
    LEAVE: 'leave',
};

module.exports = ACTIONS;

--- File Index 2: src/index.js ---
import React from 'react';
import {createRoot} from 'react-dom/client'
import './index.css';
import App from './App';
// import { ContextProvider } from './Context';
import 'bootstrap/dist/css/bootstrap.min.css';

const root_element = document.getElementById('root');
const root= createRoot(root_element)
root.render(<App />);

// root.render(
//     <React.StrictMode>
//         {/* <ContextProvider> */}
//             <App/>
//         {/* </ContextProvider> */}
//     </React.StrictMode>
// );


--- File Index 3: src/socket.js ---
import { io } from 'socket.io-client';

export const initSocket = async () => {
    const options = {
        'force new connection': true,
        reconnectionAttempt: 'Infinity',
        timeout: 10000,
        transports: ['websocket'],
    };
    // console.log('conecting to socket');
    // return io('https://c2f-api-vineethkumarm.koyeb.app:8000', options);
    return io('https://CodeCamapi.onrender.com', options);
    // return io('https://amazing-croissant-5bbe89.netlify.app/', options);
    // return io('https://c2f-api.el.r.appspot.com', options);
    // return io('http://localhost:3007',options);
    // return io()
    console.log(process.env);
    return io(process.env.REACT_APP_BACKEND_URI, options);
};

--- File Index 4: src/App.js ---
import './App.css';
import React,{createContext, useReducer} from 'react';
import {Route , BrowserRouter as Router,Routes} from 'react-router-dom'
import { initialState, reducer } from './reducers/userReducer';
import 'react-bootstrap'
//components and pages
import MyNavbar from './components/navbar';
import Home from './pages/home';
import NotFound from './pages/NotFound';
// import Testing from './pages/testing';
import { Toaster } from 'react-hot-toast';
import Slides from './pages/slides';
import AddSlides from './pages/addSlides';
import PeerCall from './pages/peerCall';
import Motivation from './pages/motivation';
import Features from './pages/features';
import Footer from './components/footer';

export const UserContext = createContext();

function App() {


	const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <UserContext.Provider value={{state,dispatch}}>
            <div>
                <Toaster 
                    position='top-right'
                />
            </div>
            <Router >
                <div className='wrapper' >

                <div className='App'>
                    <MyNavbar/>
                    <div className='content d-flex'>
                        <Routes>
                                <Route eaxct path="/" element={<Home/>} />
                                {/* <Route exact path="/call/:roomId" element={<CallPage/>} /> */}
                                <Route exact path="/call/:roomId" element={<PeerCall/>} />
                                <Route exact path='/slides' element={<Slides/>} />
                                <Route exact path='/add' element={<AddSlides/>} />
                                {/* <Route path="/chat" element={<Chat/>} /> */}
                                <Route exact path='/motivation' element={<Motivation />} />
                                {/* <Route exact path='/features' element={<Features />} /> */}
                                <Route path="*" element={<NotFound/>} />
                        </Routes>
                    </div>
                </div>
                    {/* <Footer /> */}
                </div>
            </Router>
         </UserContext.Provider>
		
	
	);
}

export default App;


--- File Index 5: src/components/navbar.js ---
import React,{ useContext } from "react";
import { Link } from "react-router-dom";
import { UserContext } from "../App";
import { useNavigate } from "react-router-dom";
import { NavbarBrand } from "react-bootstrap";
import "../styles/navbar.css"

const Navbar = () => {
	let { state, dispatch } = useContext(UserContext);
	const history = useNavigate()
	let jwt = localStorage.getItem("jwt")
	if(jwt) {
		state=true;
	}


	const renderList = () => {
		if (state) {
			return [
				<>
					<Link key='profile' className="navlink" to="/profile">Profile</Link>
					<Link key='dsf' className="navlink" to="/newPost">new post</Link>
					<button key={jwt.slice(0,3)} className="btn btn-primary" onClick={() => {
							localStorage.clear();
							dispatch({type:"CLEAR"})
							history('/')
						}
						}>
						Logout
					</button>
				
				</>
			];
		} else {
			return [
				<>
                    {/* <Link to='/testing' className="navlink">Test</Link>  */}
					{/* <Link key='login' className="navlink" to="/login">Login</Link> */}
					{/* <Link key='register' className="navlink" to="/register">Register</Link> */}
					<Link key='slides' className="navlink" to="/motivation">Motivation</Link>
					{/* <Link key='slides' className="navlink" to="/features">Features</Link> */}
					<Link key='slides' className="navlink" to="/slides">Revision</Link>
					
				</>,
			];
		}
	};

	return (
		<nav className="main-nav">
			{/* <div className="logo" ><h3 href="/">CodeCam</h3></div> */}
            <NavbarBrand className="logo">
                <Link key='logo' to="/">
                    CodeCam
                    {/* Test */}
                </Link>
            </NavbarBrand>
			<div className="navlinks">{renderList()}</div>
		</nav>
	);
};

export default Navbar;

--- File Index 6: src/components/peditor.js ---
import React, { useEffect, useState, useRef, useMemo } from 'react'
import { useLocation } from 'react-router-dom';
import { Button, Form } from 'react-bootstrap';
// codemirror components
import { useCodeMirror } from '@uiw/react-codemirror';
import Select from 'react-select';

// import languages 
import { javascript } from '@codemirror/lang-javascript';
import { cpp } from '@codemirror/lang-cpp';
import { python } from '@codemirror/lang-python';
import { java } from '@codemirror/lang-java';

// import themes 
import { githubDark } from '@uiw/codemirror-theme-github';
import { xcodeDark, xcodeLight } from '@uiw/codemirror-theme-xcode';
import { eclipse } from '@uiw/codemirror-theme-eclipse';
import { abcdef } from '@uiw/codemirror-theme-abcdef';
import { solarizedDark } from '@uiw/codemirror-theme-solarized';
import "../styles/editor.css"
import { toast } from 'react-hot-toast';

var qs = require('qs');


// const extensions = [javascript()]


const Editor = ({ sendHandler, roomId, onCodeChange, code, lang }) => {
    const history = useLocation()
    const location = useLocation()
    const uname = location?.state?.username
    const [theme, setTheme] = useState(githubDark);
    // const [code, se==1ode] = useState(code);
    const [selectValue, setSelectValue] = useState('javascript')
    const [extensions, setExtensions] = useState([javascript()])
    const [placeholder, setPlaceholder] = useState('Please enter the code.');
    const [input, setInput] = useState('')
    const [output, setOutput] = useState('')
    const [ran, setran] = useState(false)
    const [tc, setTc] = useState(true)
    const thememap = new Map()
    const langnMap = new Map()

    const editorRef = useRef(null)
    const { setContainer } = useCodeMirror({
        container: editorRef.current,
        extensions,
        value: code,
        theme,
        editable: true,
        height: `70vh`,
        width: `45vw`,
        basicSetup: {
            foldGutter: false,
            dropCursor: false,
            indentOnInput: false,
        },
        options: {
            autoComplete: true,
        },
        placeholder: placeholder,
        style: {
            position: `relative`,
            zIndex: `999`,
            borderRadius: `10px`,
        },
        onChange: (value) => {
            onCodeChange(value)
            sendHandler(3, { value })
        }

    })


    const themeInit = () => {
        thememap.set("githubDark", githubDark)
        thememap.set("xcodeDark", xcodeDark)
        thememap.set("eclipse", eclipse)
        thememap.set("xcodeLight", xcodeLight)
        thememap.set("abcdef", abcdef)
        thememap.set("solarizedDark", solarizedDark)
    }

    const langInit = () => {
        langnMap.set('java', java)
        langnMap.set('cpp', cpp)
        langnMap.set("javascript", javascript)
        langnMap.set('python', python)
    }


    const handleThemeChange = (event) => {
        setTheme(thememap.get(event.value));
    };

    const handleLanguageChange = (event) => {
        setExtensions([langnMap.get(event.value)()]);

        sendHandler(4, { newLang: event.value })
        setSelectValue(event.value)
    };

    themeInit()
    langInit()

    function langCode(e) {
        if (e == 'javascript') return 'js';
        else if (e == 'python') return 'py';
        return e;
    }

    useEffect(() => {
        if (!editorRef.current) {
            alert('error loading editor')
            history('/')
        }
        if (editorRef.current) {
            setContainer(editorRef.current)
        }

    }, [editorRef.current])

    useEffect(() => {
        if (lang != selectValue && lang) {
            setSelectValue(lang)
            setExtensions([langnMap.get(lang)()]);
        }
    }, [lang])

    const compile = (e) => {
        e.preventDefault();
        var data = qs.stringify({
            'code': code,
            'language': langCode(selectValue),
            'input': input
        });
        var config = {
            method: 'post',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            },
            body: data
        };
        fetch('https://api.codex.jaagrav.in', config)
            .then(res => res.json())
            .then(data => {
                if (data['error'].length == 0) {
                    setTc(true)
                    toast.success("compiled sucessfully")
                    setOutput(data['output'])
                }
                else {
                    setTc(false)
                    toast.error("compilation error")
                    setOutput(data['error'])
                }
            })
            .catch(function (error) {
                console.log(error);
            });
    }

    const handleInputChange = (e) => {
        setInput(e.target.value)
    }
    const handleOutputChange = (e) => {
        setOutput(e.target.value)
    }

    const themeOptions = [
        { value: "githubDark", label: "Github Dark" },
        { value: "eclipse", label: "Eclipse" },
        { value: "xcodelight", label: "X Code Light" },
        { value: "xcodedark", label: "X Code Dark" },
        { value: "solarizeddark", label: "Solarized Dark" },
        { value: "abcdef", label: "ABCDEFs" },
    ]

    const langOptions = [
        { value: "javascript", label: "JavaScript" },
        { value: "java", label: "Java" },
        { value: "cpp", label: "C++" },
        { value: "python", label: "Python" }
    ]

    return (
        <div className='editorcomponent'>
            <div style={{
                display: "flex",
                gap: "10px",
                flexDirection: "column"
            }}>
                <div>
                    <span>Theme:</span>
                    <Select
                        onChange={handleThemeChange}
                        classNamePrefix="select"
                        defaultValue={themeOptions[0]}
                        name="color"
                        options={themeOptions}
                    />
                </div>
                <div>
                    <span>Language:</span>
                    <Select
                        onChange={handleLanguageChange}
                        className="basic-single"
                        classNamePrefix="select"
                        defaultValue={langOptions[0]}
                        name="color"
                        options={langOptions}
                    />
                </div>
                <button className='run ' onClick={compile} >Run</button>
            </div>
            <div style={{marginTop: "20px"}} ref={editorRef} className='ide' ></div>
            <div style={{display: "flex", flexDirection: "row", justifyContent: "space-between", marginTop: "20px"}} className='iodiv d-flex flex-wrap'>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Input</Form.Label>
                    <Form.Control as="textarea" rows={2} value={input} onChange={handleInputChange} />
                </Form.Group>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Output</Form.Label>
                    <Form.Control as="textarea" rows={2} value={output} className={tc ? 'ctxt' : 'etxt'} onChange={handleOutputChange} disabled />
                </Form.Group>

            </div>
        </div>
    );
}

export default Editor

--- File Index 7: src/components/footer.js ---
import React from 'react'
import "../styles/footer.css"
const Footer = () => {
  return (
    <footer className="page-footer font-small bg-dark text-center text-white pt-2">

        <div className="footer-copyright text-center py-3">
            <h5>Made With ❤️ by Aaditya Kumar </h5>
        </div>

    </footer>
    )
}

export default Footer


--- File Index 8: src/components/pchat.js ---
import React, {useEffect, useState} from 'react'
import { Form } from 'react-bootstrap'

import "../styles/chat.css"

const Chat = ({sendHandler, roomId, username}) => {

    const [msg, setmsg] = useState('')

    const handleChange = (e) => {
        setmsg(e.target.value)
    }
    const handleSend = (e) => {
        e.preventDefault();
        if(msg.length==0) return;
        sendHandler(2,{username, msg})
        addSendMsg(msg)
        setmsg('')
    }
    
    const addSendMsg = (text) => {
        const element = 
            ` <div class='send'>
                    <div class='msg'>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML=element
        rdiv.setAttribute('class', 'msg-container')
        
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)  
        par.scrollTop = par.scrollHeight  
    }


    return (
        <div className='chatCont mt5'>
            <div className='chatBox'>
                <div className='scroller' id='msg-div'>
                </div>
                <div className='minp'>
                    <Form onSubmit={handleSend}>

                    
                        <Form.Control
                            type="text" 
                            placeholder="Enter message" 
                            name="msg"
                            onChange={handleChange}
                            value={msg}
                            className='finp inlin'
                        />
                        {/* <Button className='inlin' onClick={handleSend}> Send</Button> */}
                    </Form>
                </div>
            </div>
        </div>
    )
}

export default Chat

--- File Index 9: src/pages/NotFound.js ---
import React from 'react'

const NotFound = () => {
  return (
    <div className='errorpage'><span>Sorry, the request resources are not available on this website</span></div>
  )
}

export default NotFound

--- File Index 10: src/pages/features.js ---
import React from 'react'

const Features = () => {
  return (
    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Features</h2>
        <ul>
            <li>Video Chatting</li>
            <li>Screen Sharing</li>
            <li>Face Monitoring</li>
            <li>Collaborative Code Editor</li>
            <li>Online Compiler</li>
        </ul>
    </div>
  )
}

export default Features

--- File Index 11: src/pages/motivation.js ---
import React from 'react'

const Motivation = () => {
  return (

    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Motivation</h2>
        <p>Introducing the ultimate solution for seamless and efficient online interviews - this innovative 
            project brings together the power of video chat, code editor, and compiler all in one platform!
             No more juggling between multiple tabs or applications during interviews. With this website, you 
             can now focus solely on showcasing your skills and acing those coding challenges. Embrace the 
             ease of navigating through the interview process effortlessly, while impressing your potential 
             employers with your coding prowess.

        </p>
    </div>
  )
}

export default Motivation

--- File Index 12: src/pages/home.js ---
import React, { useState } from 'react'
import {  Form, Button } from 'react-bootstrap'
import {useNavigate} from 'react-router-dom'
import ShortUniqueId from 'short-unique-id';
import toast from 'react-hot-toast'
import copy from 'copy-to-clipboard';
import { Carousel } from 'react-bootstrap';
import {InfoCircleFill} from 'react-bootstrap-icons'
import Footer from '../components/footer';

const Home = () => {
    const Suid = new ShortUniqueId()
    const history = useNavigate()
    const valt = 'Enter the Session Id', invalt = 'Invalid Session  Id';
    const lenvalt = 'Enter only 10 characters Session Id';
    const [uname, setuname] = useState('')    
    const [fclick, setFclick] = useState(false)
    const [sid, setSid] = useState('')
    const [sid1, setSid1] = useState('')
    const [validText, setvalidText] = useState(valt)
    const alphanumericRegex = /^[a-zA-Z0-9]+$/;
    const [interviewer, setInterviewer] = useState(false)
  

    const handleChange1 = (e) => {
        const input = e.target.value
        if (input && !alphanumericRegex.test(input)) {
            setvalidText(invalt)
            setSid1('')
            return;
        } 
        setvalidText(lenvalt)
        if(input.length>10) {
            setSid1('')
            return
        }
        setSid1(input)
    }

    const handleNameChange = (e) => {
        setuname(e.target.value)
    }

    const generateCode = (e) => {
        e.preventDefault();
        setFclick(true)
        let code  = Suid(10);
        setSid(code)
        setSid1(code)
        if (copy(code))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    const handleSwitch = (e) => {
        setInterviewer(!interviewer)
        // console.log(interviewer);
    }

    const handleJoin = async (e)=> {
        e.preventDefault()
        if(sid.length===0 && sid1.length===0) {
            toast('Please generate or fill the Session ID', {
                icon: '❕',
              });
            return;
        }
        if(uname.length==0) {
            toast.error("Name field is empty")
            return;
        }
        const roomId = sid.length>0?sid:sid1
        if(sid.length>0) {
            localStorage.setItem('init', 'true')
        }
        toast.success('Joining new Call')
        history(`/call/${roomId}`, {
            state: {
                username: uname,
                interviewer:!interviewer,
            },
        });
    }
    
    return (
        <>
        <div className='main-home'>
            <div className='cen-container'>
                {/* <Image src='/qna.png' className='img-fluid' /> */}
                <div className='lcentral l-shadow'>
                <Carousel data-bs-theme="dark" className='fwid'>
                    <Carousel.Item key='1' interval={500}>
                        <img
                        className="d-block w-100 img-fluid"
                        src="/qna.png"
                        alt="First slide"
                        />
                        <Carousel.Caption>
                        <h5 className='blk-txt'>Online Interviews</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='2' interval={750}>
                        <img alt='face motion' className="d-block w-100 img-fluid" src='/face scanner.png'/>
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Face Motion Detection</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='3' interval={750}>
                        <img alt='video' className="d-block w-100 img-fluid"  src="/interview.jpg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Video Interview</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='4' interval={750}>
                        <img alt='code editor' className="d-block w-100 img-fluid"  src="/editor.jpeg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Live Code Editor</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='5' interval={750}>
                        <img alt='live chat' className="d-block w-100 img-fluid"  src="/chat.png" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'> Live Chat</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    
                </Carousel>
                </div>
            </div>
            <div className='cen-container'>
                <div className='central r-shadow'>
                    <div className='d-flex flex-column'>
                        <div className='jcontent'>
                            <h3 className='t-color'>Welcome to <span className='myfont'>CodeCam</span></h3>

                        </div>
                        <div className='jcontent mt4'>
                            <h4>Start a New Session</h4>
                            {/* <Form> */}
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    
                                    <Form.Control 
                                        className='genInp' 
                                        type="text" 
                                        placeholder="#uniqueId" 
                                        disabled 
                                        name="sid"
                                        value={sid}
                                    />
                                        <Button className="btn-dark gen-btn"
                                            onClick={generateCode}
                                        >Generate {fclick ? 'Again' : ''}</Button>
                                </Form.Group>
                                <h4>or</h4>
                            
                        </div>
                        <div className='jcontent mt-4'>
                            <h4>Join with Code</h4>
                                <Form.Group className="mmb fin " >
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Id" 
                                        name="sid1"
                                        onChange={handleChange1}
                                        value={sid1}
                                        
                                    />
                                    <Form.Text>{validText}</Form.Text>
                                </Form.Group>
                        </div>
                        <div className='jcontent'>
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Enter your good Name" 
                       
                                        name="uname"
                                        onChange={handleNameChange}
                                        value={uname}
                                    />
                                </Form.Group>
                                <Form.Group className='ml-2'>
                                    <Form.Check
                                        type="switch"
                                        id="custom-switch"
                                        label="Join as Interviewer "
                                        value={interviewer}
                                        name='interviewer'
                                        onChange={handleSwitch}
                                        style={{display: 'inline', textAlign: 'left !important', padding: '5px'}}
                                    />
                                    <InfoCircleFill className='infoIcon'></InfoCircleFill>
                                    <span className="afterHoverText">Toggle to disable Face Monitoring</span>
                                </Form.Group>
                                <Form.Group className='mt-2'>
                                    <Button onClick={handleJoin}>Join Call</Button>
                                </Form.Group>

                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div className='disclaimer'>
            <h5>Note:</h5>
            <p>Sit in well lit room and Face camera to avoid misbehaviour notifications. <br></br>
            If your video is not visible, hard reload the page and rejoin.
            </p>
        </div>
        </>
    )
}

export default Home

--- File Index 13: src/pages/peerCall.js ---
import React, { useState, createContext, useRef, useEffect } from "react";

import toast from 'react-hot-toast';
import { useLocation, useNavigate, useParams, Navigate } from 'react-router-dom';
import { Peer } from 'peerjs'
import { Button } from "react-bootstrap";
import * as faceapi from 'face-api.js'
import Chat from "../components/pchat";
import copy from 'copy-to-clipboard';
import Editor from "../components/peditor";
import "../styles/callpage.css";
const server = process.env.REACT_APP_BACKEND_URI;
const server_host = process.env.REACT_APP_BACKEND_HOST;
const PeerCall = () => {
    const location = useLocation();
    const history = useNavigate();
    const { roomId } = useParams();

    const myName = location.state?.username
    const interviewer = location.state?.interviewer

    const MOTION_THRESHOLD = 40;
    // const myConns = new Map()
    const [myConns, setMyConns] = useState(new Map())
    const [mycalls, setMyCalls] = useState(new Set())
    const idName = new Map()


    var userMoves = 0;
    var previousLandmarks = null;
    var warned = false;
    var peer = null
    var dataStream = null
    var screenStream = null
    var myPeerId;
    var timer;

    const [allPeers, setAllPeers] = useState([])
    const [stream, setstream] = useState(null)

    const [lang, setLang] = useState('javascript')
    const [chatState, setChatState] = useState(false)
    const [displayCheck, setDisplayCheck] = useState(false)
    const [sirId, setsirId] = useState(null)
    const myVideo = useRef()
    const [code, setcode] = useState(`console.log("Live shared Code Editor")`)

    //Face motion logic
    function analyzeFaceMotions(landmarks) {
        const currentLandmarks = landmarks._positions;

        if (previousLandmarks && currentLandmarks.length == 68) {
            let totalMotion = 0, averageMotion = 0;
            for (let i = 0; i < 68; i++) {
                const dx = currentLandmarks[i]._x - previousLandmarks[i]._x;
                const dy = currentLandmarks[i]._y - previousLandmarks[i]._y;
                const distance = Math.sqrt(dx * dx + dy * dy)
                totalMotion += distance
            }
            averageMotion = totalMotion / 68;
            // console.log(averageMotion);
            if (averageMotion > MOTION_THRESHOLD) {
                toast('Face motion detected, Please concentrate on the interview', {
                    icon: '❕',
                });
                userMoves++;
            }
        }
        previousLandmarks = currentLandmarks;
    }

    async function detectFaceMotions() {
        if (!myVideo.current) return;

        timer = setInterval(async () => {
            if (warned) {
                if (sirId) { }
                // socketRef?.current?.emit(ACTIONS.BEHAVIOUR , {roomId})
                else {
                    toast.error('Please Behave until the Interviewer joins')
                }
                warned = false
                userMoves = 0
                return
            }
            if (userMoves > 12) {
                toast.error("Warning! Interviewer will be notified if movement is observed again.")
                userMoves = userMoves - 2;
                warned = true;
            }
            const detections = await faceapi.detectAllFaces(myVideo.current,
                new faceapi.TinyFaceDetectorOptions())
                .withFaceLandmarks()

            if (myVideo.current && detections.length == 0) {
                toast.error("Please sit in a well lit room and face the Webcam!")
                alert('Face cannot be detected ! Please sit in a weel lit area.')
                userMoves++;
            }
            else if (detections?.length > 1) {
                toast.error(`${detections.length} persons spotted in camera`)
                userMoves++;
            }
            else if (detections && detections.length == 1) {
                analyzeFaceMotions(detections[0].landmarks)
            } else {
                console.log(detections);
                toast.error(" Please face the webcam!")
            }

        }, 2500)
    }

    useEffect(() => {
        //take camera permission
        navigator?.mediaDevices?.getUserMedia({ video: true, audio: true })
            .then(videoStream => {
                dataStream = videoStream;
                setstream(videoStream);

                // initialize socket
                init(videoStream)

            })
            .catch((error) => {
                console.log(error);
                toast.error('Cannot connect without video and audio permissions')
                return <Navigate to="/" />;
            });
        //load faceapi Models
        loadModels()
        //socket connecting function
        const init = async (videoStream) => {
            // PeerJS functionality starts 
            var peer_params = {
                host: server_host,
                path: '/myapp',
            }
            if (server_host == 'localhost') peer_params['port'] = 3007
            peer = new Peer(peer_params)
            // once peer created join room
            peer.on('open', function (id) {
                myPeerId = id

                addVideo(videoStream, id, myName)
                if (interviewer)
                    detectFaceMotions()
                let flag = interviewer ? false : true
                fetch(`${server}join`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        username: myName,
                        peerId: id,
                        flag
                    })
                }).then(res => res.json())
                    .then(res_arr => {
                        const { data } = res_arr
                        setAllPeers(data);
                        for (const { peerId, username } of data) {

                            let conn = peer.connect(peerId)

                            conn.on("open", () => {
                                myConns.set(conn, username)
                                conn.send({ sig: 1, data: { id, name: myName } })
                                let call = peer.call(peerId, videoStream)
                                call.on('stream', (remoteStream) => {
                                    mycalls.add(call)
                                    addVideo(remoteStream, peerId, username)
                                })
                            })

                            conn.on('data', ({ sig, data }) => {
                                recvHandler(conn, sig, data)
                            })

                            conn.on('close', () => {
                                myConns.delete(conn)
                                const element = document.getElementById(peerId);
                                element?.remove();
                            })

                            if (peerId == sirId) {
                                toast.success('Interviewer Joined')
                            }
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    })

            });

            peer.on("connection", (conn) => {
                conn.on('data', ({ sig, data }) => {
                    recvHandler(conn, sig, data)
                })
                conn.on('close', () => {
                    myConns.delete(conn)
                    toast.success(`${idName[conn.peer]} left the Call`)
                    const element = document.getElementById(conn.peer);
                    element?.remove();
                })
            });

            peer.on('call', (call) => {
                call.answer(videoStream)
                mycalls.add(call)
                call.on('stream', (remoteStream) => {
                    addVideo(remoteStream, call.peer, idName[call.peer])
                })
                toast.success(`${idName[call.peer]} joined the Call`)
            })
        };

        // Clean up function to remove camera permissions and end socket
        return () => {
            const ele = document.getElementById(myPeerId)
            ele?.remove()
            clearInterval(timer)
            if (peer) {
                try {
                    fetch(`${server}leave`, {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json"
                        },
                        body: JSON.stringify({
                            roomId,
                            peerId: myPeerId,
                        })
                    })
                } catch (err) {
                    console.log(err);
                    toast.error("Coudn't leave the room at the current moment")
                }
                peer.destroy();
            }
            if (dataStream) {
                const tracks = dataStream.getTracks();
                tracks.forEach((track) => track.stop());
            }
        };
    }, []);

    //Funtions to be triggered by child components passed as props

    const sendHandler = (sig, data) => {
        for (const [conn, name] of myConns) {
            conn.send({ sig, data })
        }
    }

    const recvHandler = (conn, sig, data) => {
        switch (sig) {
            case 1: {
                //share name
                const { id, name } = data
                myConns.set(conn, name)
                idName[id] = name
                conn.send(3, { value: code })
                break;
            }
            case 2: {
                // chat msg
                const { username, msg } = data
                addRecvMsg(msg, username)
            }
            case 3: {
                // code update
                const { value } = data
                console.log(value);
                if (value != code)
                    setcode(value);
            }
            case 4: {
                // language change
                const { newLang } = data
                if (lang != newLang) setLang(newLang)
            }
            default:
                break;
        }
    }

    const addRecvMsg = (text, name = 'Unknown') => {
        setChatState(true)
        const element =
            ` <div class='receive'>
                    <div class='msg'>
                    <span class='senderName' >${name}</span>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML = element
        rdiv.setAttribute('class', 'msg-container')
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)
        if (par)
            par.scrollTop = par.scrollHeight
    }


    //Functions related to this component
    function addVideo(vstream, peerID, userName = "user") {
        const prev = document.getElementById(peerID)
        prev?.remove()
        const row = document.createElement('div')
        row.setAttribute('class', 'mt-2')
        row.setAttribute('id', peerID)

        const video = document.createElement('video')
        video.srcObject = vstream;
        video.addEventListener('loadedmetadata', () => {
            video.play()
        })
        // console.log(peerID, myPeerId);
        if (peerID == myPeerId) {
            video.muted = true
            myVideo.current = video
        }

        const span = document.createElement('span')
        span.innerText = userName
        span.setAttribute('class', 'tagName')

        row.append(video)
        row.append(span)

        const exist = document.getElementById(peerID)
        if (exist) return

        const peerDiv = document.getElementById('peerDiv')

        peerDiv?.insertBefore(row, peerDiv.children[0])

    }

    const loadModels = () => {
        Promise.all([
            faceapi.nets.faceLandmark68Net.loadFromUri("/models"),
            faceapi.nets.tinyFaceDetector.loadFromUri("/models"),
            faceapi.loadFaceLandmarkTinyModel("/models")
        ]).then(() => {
            // console.log('Models Loaded');
        }).catch(err => {
            console.log('FaceAPI modules loading error', err);
        })

    }

    const screenShareHandler = async (e) => {
        e.preventDefault()
        setDisplayCheck(!displayCheck)
        idName['screen'] = 'screen'
        navigator?.mediaDevices?.getDisplayMedia({ audio: true }).then(displStream => {
            screenStream = displStream
            addVideo(displStream, 'screen', 'Screen')
            replaceStream(displStream)
        }).catch((error) => {
            console.log(error);
        });

    }

    const replaceStream = (mediaStream) => {
        for (const call of mycalls) {
            for (let sender of call.peerConnection?.getSenders()) {
                if (sender.track.kind == "audio") {
                    if (mediaStream.getAudioTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getAudioTracks()[0]);
                    }
                }
                if (sender.track.kind == "video") {
                    if (mediaStream.getVideoTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getVideoTracks()[0]);
                    }
                }
            }
        }
    }


    function stopCapture(evt) {
        evt.preventDefault()
        setDisplayCheck(!displayCheck)
        replaceStream(stream)
        let ele = document.getElementById('screen')
        const evid = ele?.childNodes[1];
        if (evid && evid.srcObject) {
            const tracks = evid.srcObject.getTracks();
            tracks.forEach((track) => track.stop());
        }
        ele?.remove();
    }
    const copyCode = (e) => {
        e.preventDefault();
        if (copy(roomId))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    function chatHider() {
        setChatState(chatState => !chatState)
    }

    function leaveRoom() {
        if (peer) {
            try {
                fetch(`${server}leave`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        peerId: myPeerId,
                    })
                })
            } catch (err) {
                console.log(err);
                toast.error("Coudn't leave the room at the current moment")
            }
            peer.destroy()
        }
        if (dataStream) {
            const tracks = dataStream.getTracks();
            tracks.forEach((track) => track.stop());
        }
        clearInterval(timer)
        history('/');
    }

    if (!location.state) {
        return <Navigate to="/" />;
    }


    return (
        <>
            <div style={{ marginBottom: "50px", display: "flex", flexDirection: "row", justifyContent: "space-between" }} >
                <div style={{ marginTop: "20px" }}>
                    <div className='econt'>
                        <Editor
                            conns={myConns}
                            roomId={roomId}
                            onCodeChange={setcode}
                            code={code}
                            lang={lang}
                            peer={peer}
                            sendHandler={sendHandler}
                        />
                    </div>
                </div>
                <div style={{
                    display: "flex",
                    flexDirection: "column",
                    alignItems: "center",
                    width: "100%",
                    marginTop: "20px"
                }}>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        gap: "20px",
                        width: "100%",
                    }}>
                        <Button
                            onClick={leaveRoom}
                            className="mt-2 btn-danger"
                            style={{ width: '120px', margin: 'auto' }}>Leave</Button>
                        <Button
                            onClick={!displayCheck ? screenShareHandler : stopCapture} className="mt-2" style={{ width: '170px', margin: 'auto' }}>{!displayCheck ? 'Screen Share' : 'Stop Share'}</Button>
                        <Button
                            onClick={copyCode}
                            className="mt-2 btn-info"
                            style={{ width: '160px', margin: 'auto' }}>
                            Copy Session Id
                        </Button>
                        <div onClick={chatHider}>
                            {!chatState ?
                                <img style={{ width: "50px" }} src="/toggle_chat.png"></img>
                                : <></>
                            }
                        </div>
                    </div>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        marginTop: "100px",
                        width: "100%",
                        justifyContent: "space-evenly"
                    }} id="peerDiv">
                        {/* <div style={{ margin: "0px 10px" }} className='' id="users">
                        </div> */}
                    </div>
                </div>

            </div >

            {!chatState ?
                <></>
                : <>
                    <Chat
                        recvMsg={addRecvMsg}
                        conns={myConns}
                        roomId={roomId}
                        username={myName}
                        sendHandler={sendHandler}
                        peer={peer}
                        className='chatCont'
                    />
                    <img src="/close_chat.png" onClick={chatHider} className="fix_close"></img>
                </>
            }
        </>
    )
}

export default PeerCall

--- File Index 14: src/pages/addSlides.js ---
import React, {useState} from 'react'
import { Form , Button} from 'react-bootstrap'
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const AddSlides = () => {

    const [heading, setHeading] = useState("");
    const [body, setBody] = useState("");

    const handleSubmit = async (e) => {
        e.preventDefault();

        const data = {
        heading: heading,
        body: body
        };

        try {
            const response = await fetch(`${serverLink}/save_data`, {
                method: "POST",
                headers: {
                "Content-Type": "application/json"
                },
                body: JSON.stringify(data)
            });
          
            if (response.ok) {
            //   console.log("Data saved successfully");
                toast.success("Slides Updated")
            } else {
              console.error("Error saving data:", response.status);
              toast.error('Error updating')
            }
          } catch (error) {
            console.error("Error saving data:", error);
            toast.error("Error sending data")
          }
          

        // Reset form fields
        setHeading("");
        setBody("");
    };



    return (
        <div className='cen-container mt-5 lcentral l-shadow'>
            <h3>Add New Slides</h3>
            <form onSubmit={handleSubmit} >
                {/* <label htmlFor="heading">Heading:</label> */}
                {/* <input
                    type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required
                /> */}
                
                {/* <label htmlFor="body">Body:</label>
                <textarea
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                ></textarea> */}
                <Form.Group className="mb-3" >
                    <Form.Label>Heading:</Form.Label>
                    <Form.Control type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required />
                </Form.Group>
                <Form.Group className="mb-3" >
                    <Form.Label>Body:</Form.Label>
                    <Form.Control as="textarea" 
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                    rows={3} />
                </Form.Group>

                <Button type="submit">Submit</Button>
            </form>
        </div>
    )
}

export default AddSlides

--- File Index 15: src/pages/slides.js ---
import React, {useState, useEffect} from 'react'
import { Carousel } from 'react-bootstrap';
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const Slides = () => {
    let local_Data = localStorage.getItem('slides')
    const [data, setData] = useState(local_Data!=undefined ? JSON.parse(local_Data) : [])

    useEffect(() => {
        async function fetchData() {
            try {
                const res= await fetch(`${serverLink}get_data`, {
                    method: "GET",
                    headers: {
                    "Content-Type": "application/json"
                    },
                });
                const newData = await res.json();
                setData(newData)
                localStorage.setItem('slides', JSON.stringify(newData))
            } catch(error) {
                console.log(error);
                toast.error("Error receiving data")
            }
        }
        fetchData();
        
    }, [])

    return (
        <div className=''>
            <div className='cen-container l-shadow lcentral mt-5 '>
                <Carousel>
                {   
                    data.map((item, index) => (
                        
                        <Carousel.Item key={index} interval={500}>
                            <div key={index} className='f-height'>
                                <h2>{item.heading}</h2>
                                <h6>{item.body }</h6>
                            </div>
                        </Carousel.Item>

                ))}
                </Carousel>
            </div>
        </div>
    )
}

export default Slides

--- File Index 16: src/reducers/userReducer.js ---
export const initialState = null;

export const reducer = (state, action) => {
	if (action.type === "USER") {
		return action.payload;
	}if (action.type === "CLEAR") {
		return null;
	}
	return state;
};



Analyze the codebase context.
Identify the top 5-10 core most important abstractions to help those new to the codebase.

For each abstraction, provide:
1. A concise `name`.
2. A beginner-friendly `description` explaining what it is with a simple analogy, in around 100 words.
3. A list of relevant `file_indices` (integers) using the format `idx # path/comment`.

List of file indices and paths present in the context:
- 0 # README.md
- 1 # src/Actions.js
- 2 # src/index.js
- 3 # src/socket.js
- 4 # src/App.js
- 5 # src/components/navbar.js
- 6 # src/components/peditor.js
- 7 # src/components/footer.js
- 8 # src/components/pchat.js
- 9 # src/pages/NotFound.js
- 10 # src/pages/features.js
- 11 # src/pages/motivation.js
- 12 # src/pages/home.js
- 13 # src/pages/peerCall.js
- 14 # src/pages/addSlides.js
- 15 # src/pages/slides.js
- 16 # src/reducers/userReducer.js

Format the output as a YAML list of dictionaries:

```yaml
- name: |
    Query Processing
  description: |
    Explains what the abstraction does.
    It's like a central dispatcher routing requests.
  file_indices:
    - 0 # path/to/file1.py
    - 3 # path/to/related.py
- name: |
    Query Optimization
  description: |
    Another core concept, similar to a blueprint for objects.
  file_indices:
    - 5 # path/to/another.js
# ... up to 10 abstractions
```
2025-05-16 17:50:45,152 - INFO - PROMPT: 
For the project `CollabCam`:

Codebase Context:
--- File Index 0: README.md ---
# Welcome to CollabCam

**CollabCam** is a dynamic web platform specifically built to facilitate **online coding interviews**. Designed to streamline the interview process, CollabCam allows interviewers to engage with candidates in real-time through a collaborative environment that integrates coding, monitoring, and communication tools.

With CollabCam, interviewers can initiate a new session, share the unique session code with candidates, and dive into an interactive interview experience that mirrors the dynamics of an in-person coding test.

## Key Features

- ### **Collaborative Code Editor**
  CollabCam features a **live coding editor** where candidates can write, edit, and debug code directly in the interview session. The shared editor updates in real-time, allowing interviewers to observe the candidate’s approach, provide feedback, and guide them through technical questions or coding challenges seamlessly.

- ### **Video Monitoring**
  To ensure integrity and focus, CollabCam includes a **video monitoring tool** that tracks head movements of the candidate. When a candidate turns on their video, CollabCam will monitor and detect excessive head movements:
  - If notable movement is being detected, CollabCam issues a **warning**.
  - Continued movement post-warning may result in the **session being terminated**, allowing interviewers to maintain a controlled environment.

- ### **In-Session Chat**
  CollabCam also provides a built-in **chat feature** for smooth communication between the interviewer and candidate. Whether discussing the task at hand, providing hints, or addressing technical issues, the chat function ensures clear and effective interaction throughout the session.

> **Note**: Currently, CollabCam’s functionality is optimized for users connected on the same network due to limitations with the current PeerJS implementation. The development roadmap includes plans to expand this capability, enabling users to connect from any network globally.

## Future Scope

We have exciting plans for future updates that will enhance CollabCam’s utility:
- **Session Management**: Enable interviewers to conduct consecutive interviews without needing to reset the platform, making back-to-back interviews seamless.
- **Scoring and Feedback**: Add functionality for interviewers to assign scores, write feedback, and maintain a log of candidate performance over multiple sessions.

## Get Started

You can try out CollabCam and explore its features by visiting the link below:

[Access CollabCam](https://collabcam.onrender.com/)

---

With CollabCam, conducting remote technical interviews is simpler, more efficient, and fully interactive, providing a comprehensive environment for both interviewers and candidates.


--- File Index 1: src/Actions.js ---
const ACTIONS = {
    JOIN: 'join',
    JOINED: 'joined',
    DISCONNECTED: 'disconnected',
    CODE_CHANGE: 'code-change',
    SYNC_CODE: 'sync-code',
    SHARE_PEER_IDS: 'share-peer-ids',
    SIR_JOined: 'interviewer-joined',
    MONITOR: 'monitor',
    BEHAVIOUR: 'mis-behaving',
    SEND_MSG : 'send-message',
    RECV_MSG : 'receive-message',
    LANG_CHANGE: 'change-language',
    UPDATE_LAN: 'update-language',
    LEAVE: 'leave',
};

module.exports = ACTIONS;

--- File Index 2: src/index.js ---
import React from 'react';
import {createRoot} from 'react-dom/client'
import './index.css';
import App from './App';
// import { ContextProvider } from './Context';
import 'bootstrap/dist/css/bootstrap.min.css';

const root_element = document.getElementById('root');
const root= createRoot(root_element)
root.render(<App />);

// root.render(
//     <React.StrictMode>
//         {/* <ContextProvider> */}
//             <App/>
//         {/* </ContextProvider> */}
//     </React.StrictMode>
// );


--- File Index 3: src/socket.js ---
import { io } from 'socket.io-client';

export const initSocket = async () => {
    const options = {
        'force new connection': true,
        reconnectionAttempt: 'Infinity',
        timeout: 10000,
        transports: ['websocket'],
    };
    // console.log('conecting to socket');
    // return io('https://c2f-api-vineethkumarm.koyeb.app:8000', options);
    return io('https://CodeCamapi.onrender.com', options);
    // return io('https://amazing-croissant-5bbe89.netlify.app/', options);
    // return io('https://c2f-api.el.r.appspot.com', options);
    // return io('http://localhost:3007',options);
    // return io()
    console.log(process.env);
    return io(process.env.REACT_APP_BACKEND_URI, options);
};

--- File Index 4: src/App.js ---
import './App.css';
import React,{createContext, useReducer} from 'react';
import {Route , BrowserRouter as Router,Routes} from 'react-router-dom'
import { initialState, reducer } from './reducers/userReducer';
import 'react-bootstrap'
//components and pages
import MyNavbar from './components/navbar';
import Home from './pages/home';
import NotFound from './pages/NotFound';
// import Testing from './pages/testing';
import { Toaster } from 'react-hot-toast';
import Slides from './pages/slides';
import AddSlides from './pages/addSlides';
import PeerCall from './pages/peerCall';
import Motivation from './pages/motivation';
import Features from './pages/features';
import Footer from './components/footer';

export const UserContext = createContext();

function App() {


	const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <UserContext.Provider value={{state,dispatch}}>
            <div>
                <Toaster 
                    position='top-right'
                />
            </div>
            <Router >
                <div className='wrapper' >

                <div className='App'>
                    <MyNavbar/>
                    <div className='content d-flex'>
                        <Routes>
                                <Route eaxct path="/" element={<Home/>} />
                                {/* <Route exact path="/call/:roomId" element={<CallPage/>} /> */}
                                <Route exact path="/call/:roomId" element={<PeerCall/>} />
                                <Route exact path='/slides' element={<Slides/>} />
                                <Route exact path='/add' element={<AddSlides/>} />
                                {/* <Route path="/chat" element={<Chat/>} /> */}
                                <Route exact path='/motivation' element={<Motivation />} />
                                {/* <Route exact path='/features' element={<Features />} /> */}
                                <Route path="*" element={<NotFound/>} />
                        </Routes>
                    </div>
                </div>
                    {/* <Footer /> */}
                </div>
            </Router>
         </UserContext.Provider>
		
	
	);
}

export default App;


--- File Index 5: src/components/navbar.js ---
import React,{ useContext } from "react";
import { Link } from "react-router-dom";
import { UserContext } from "../App";
import { useNavigate } from "react-router-dom";
import { NavbarBrand } from "react-bootstrap";
import "../styles/navbar.css"

const Navbar = () => {
	let { state, dispatch } = useContext(UserContext);
	const history = useNavigate()
	let jwt = localStorage.getItem("jwt")
	if(jwt) {
		state=true;
	}


	const renderList = () => {
		if (state) {
			return [
				<>
					<Link key='profile' className="navlink" to="/profile">Profile</Link>
					<Link key='dsf' className="navlink" to="/newPost">new post</Link>
					<button key={jwt.slice(0,3)} className="btn btn-primary" onClick={() => {
							localStorage.clear();
							dispatch({type:"CLEAR"})
							history('/')
						}
						}>
						Logout
					</button>
				
				</>
			];
		} else {
			return [
				<>
                    {/* <Link to='/testing' className="navlink">Test</Link>  */}
					{/* <Link key='login' className="navlink" to="/login">Login</Link> */}
					{/* <Link key='register' className="navlink" to="/register">Register</Link> */}
					<Link key='slides' className="navlink" to="/motivation">Motivation</Link>
					{/* <Link key='slides' className="navlink" to="/features">Features</Link> */}
					<Link key='slides' className="navlink" to="/slides">Revision</Link>
					
				</>,
			];
		}
	};

	return (
		<nav className="main-nav">
			{/* <div className="logo" ><h3 href="/">CodeCam</h3></div> */}
            <NavbarBrand className="logo">
                <Link key='logo' to="/">
                    CodeCam
                    {/* Test */}
                </Link>
            </NavbarBrand>
			<div className="navlinks">{renderList()}</div>
		</nav>
	);
};

export default Navbar;

--- File Index 6: src/components/peditor.js ---
import React, { useEffect, useState, useRef, useMemo } from 'react'
import { useLocation } from 'react-router-dom';
import { Button, Form } from 'react-bootstrap';
// codemirror components
import { useCodeMirror } from '@uiw/react-codemirror';
import Select from 'react-select';

// import languages 
import { javascript } from '@codemirror/lang-javascript';
import { cpp } from '@codemirror/lang-cpp';
import { python } from '@codemirror/lang-python';
import { java } from '@codemirror/lang-java';

// import themes 
import { githubDark } from '@uiw/codemirror-theme-github';
import { xcodeDark, xcodeLight } from '@uiw/codemirror-theme-xcode';
import { eclipse } from '@uiw/codemirror-theme-eclipse';
import { abcdef } from '@uiw/codemirror-theme-abcdef';
import { solarizedDark } from '@uiw/codemirror-theme-solarized';
import "../styles/editor.css"
import { toast } from 'react-hot-toast';

var qs = require('qs');


// const extensions = [javascript()]


const Editor = ({ sendHandler, roomId, onCodeChange, code, lang }) => {
    const history = useLocation()
    const location = useLocation()
    const uname = location?.state?.username
    const [theme, setTheme] = useState(githubDark);
    // const [code, se==1ode] = useState(code);
    const [selectValue, setSelectValue] = useState('javascript')
    const [extensions, setExtensions] = useState([javascript()])
    const [placeholder, setPlaceholder] = useState('Please enter the code.');
    const [input, setInput] = useState('')
    const [output, setOutput] = useState('')
    const [ran, setran] = useState(false)
    const [tc, setTc] = useState(true)
    const thememap = new Map()
    const langnMap = new Map()

    const editorRef = useRef(null)
    const { setContainer } = useCodeMirror({
        container: editorRef.current,
        extensions,
        value: code,
        theme,
        editable: true,
        height: `70vh`,
        width: `45vw`,
        basicSetup: {
            foldGutter: false,
            dropCursor: false,
            indentOnInput: false,
        },
        options: {
            autoComplete: true,
        },
        placeholder: placeholder,
        style: {
            position: `relative`,
            zIndex: `999`,
            borderRadius: `10px`,
        },
        onChange: (value) => {
            onCodeChange(value)
            sendHandler(3, { value })
        }

    })


    const themeInit = () => {
        thememap.set("githubDark", githubDark)
        thememap.set("xcodeDark", xcodeDark)
        thememap.set("eclipse", eclipse)
        thememap.set("xcodeLight", xcodeLight)
        thememap.set("abcdef", abcdef)
        thememap.set("solarizedDark", solarizedDark)
    }

    const langInit = () => {
        langnMap.set('java', java)
        langnMap.set('cpp', cpp)
        langnMap.set("javascript", javascript)
        langnMap.set('python', python)
    }


    const handleThemeChange = (event) => {
        setTheme(thememap.get(event.value));
    };

    const handleLanguageChange = (event) => {
        setExtensions([langnMap.get(event.value)()]);

        sendHandler(4, { newLang: event.value })
        setSelectValue(event.value)
    };

    themeInit()
    langInit()

    function langCode(e) {
        if (e == 'javascript') return 'js';
        else if (e == 'python') return 'py';
        return e;
    }

    useEffect(() => {
        if (!editorRef.current) {
            alert('error loading editor')
            history('/')
        }
        if (editorRef.current) {
            setContainer(editorRef.current)
        }

    }, [editorRef.current])

    useEffect(() => {
        if (lang != selectValue && lang) {
            setSelectValue(lang)
            setExtensions([langnMap.get(lang)()]);
        }
    }, [lang])

    const compile = (e) => {
        e.preventDefault();
        var data = qs.stringify({
            'code': code,
            'language': langCode(selectValue),
            'input': input
        });
        var config = {
            method: 'post',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            },
            body: data
        };
        fetch('https://api.codex.jaagrav.in', config)
            .then(res => res.json())
            .then(data => {
                if (data['error'].length == 0) {
                    setTc(true)
                    toast.success("compiled sucessfully")
                    setOutput(data['output'])
                }
                else {
                    setTc(false)
                    toast.error("compilation error")
                    setOutput(data['error'])
                }
            })
            .catch(function (error) {
                console.log(error);
            });
    }

    const handleInputChange = (e) => {
        setInput(e.target.value)
    }
    const handleOutputChange = (e) => {
        setOutput(e.target.value)
    }

    const themeOptions = [
        { value: "githubDark", label: "Github Dark" },
        { value: "eclipse", label: "Eclipse" },
        { value: "xcodelight", label: "X Code Light" },
        { value: "xcodedark", label: "X Code Dark" },
        { value: "solarizeddark", label: "Solarized Dark" },
        { value: "abcdef", label: "ABCDEFs" },
    ]

    const langOptions = [
        { value: "javascript", label: "JavaScript" },
        { value: "java", label: "Java" },
        { value: "cpp", label: "C++" },
        { value: "python", label: "Python" }
    ]

    return (
        <div className='editorcomponent'>
            <div style={{
                display: "flex",
                gap: "10px",
                flexDirection: "column"
            }}>
                <div>
                    <span>Theme:</span>
                    <Select
                        onChange={handleThemeChange}
                        classNamePrefix="select"
                        defaultValue={themeOptions[0]}
                        name="color"
                        options={themeOptions}
                    />
                </div>
                <div>
                    <span>Language:</span>
                    <Select
                        onChange={handleLanguageChange}
                        className="basic-single"
                        classNamePrefix="select"
                        defaultValue={langOptions[0]}
                        name="color"
                        options={langOptions}
                    />
                </div>
                <button className='run ' onClick={compile} >Run</button>
            </div>
            <div style={{marginTop: "20px"}} ref={editorRef} className='ide' ></div>
            <div style={{display: "flex", flexDirection: "row", justifyContent: "space-between", marginTop: "20px"}} className='iodiv d-flex flex-wrap'>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Input</Form.Label>
                    <Form.Control as="textarea" rows={2} value={input} onChange={handleInputChange} />
                </Form.Group>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Output</Form.Label>
                    <Form.Control as="textarea" rows={2} value={output} className={tc ? 'ctxt' : 'etxt'} onChange={handleOutputChange} disabled />
                </Form.Group>

            </div>
        </div>
    );
}

export default Editor

--- File Index 7: src/components/footer.js ---
import React from 'react'
import "../styles/footer.css"
const Footer = () => {
  return (
    <footer className="page-footer font-small bg-dark text-center text-white pt-2">

        <div className="footer-copyright text-center py-3">
            <h5>Made With ❤️ by Aaditya Kumar </h5>
        </div>

    </footer>
    )
}

export default Footer


--- File Index 8: src/components/pchat.js ---
import React, {useEffect, useState} from 'react'
import { Form } from 'react-bootstrap'

import "../styles/chat.css"

const Chat = ({sendHandler, roomId, username}) => {

    const [msg, setmsg] = useState('')

    const handleChange = (e) => {
        setmsg(e.target.value)
    }
    const handleSend = (e) => {
        e.preventDefault();
        if(msg.length==0) return;
        sendHandler(2,{username, msg})
        addSendMsg(msg)
        setmsg('')
    }
    
    const addSendMsg = (text) => {
        const element = 
            ` <div class='send'>
                    <div class='msg'>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML=element
        rdiv.setAttribute('class', 'msg-container')
        
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)  
        par.scrollTop = par.scrollHeight  
    }


    return (
        <div className='chatCont mt5'>
            <div className='chatBox'>
                <div className='scroller' id='msg-div'>
                </div>
                <div className='minp'>
                    <Form onSubmit={handleSend}>

                    
                        <Form.Control
                            type="text" 
                            placeholder="Enter message" 
                            name="msg"
                            onChange={handleChange}
                            value={msg}
                            className='finp inlin'
                        />
                        {/* <Button className='inlin' onClick={handleSend}> Send</Button> */}
                    </Form>
                </div>
            </div>
        </div>
    )
}

export default Chat

--- File Index 9: src/pages/NotFound.js ---
import React from 'react'

const NotFound = () => {
  return (
    <div className='errorpage'><span>Sorry, the request resources are not available on this website</span></div>
  )
}

export default NotFound

--- File Index 10: src/pages/features.js ---
import React from 'react'

const Features = () => {
  return (
    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Features</h2>
        <ul>
            <li>Video Chatting</li>
            <li>Screen Sharing</li>
            <li>Face Monitoring</li>
            <li>Collaborative Code Editor</li>
            <li>Online Compiler</li>
        </ul>
    </div>
  )
}

export default Features

--- File Index 11: src/pages/motivation.js ---
import React from 'react'

const Motivation = () => {
  return (

    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Motivation</h2>
        <p>Introducing the ultimate solution for seamless and efficient online interviews - this innovative 
            project brings together the power of video chat, code editor, and compiler all in one platform!
             No more juggling between multiple tabs or applications during interviews. With this website, you 
             can now focus solely on showcasing your skills and acing those coding challenges. Embrace the 
             ease of navigating through the interview process effortlessly, while impressing your potential 
             employers with your coding prowess.

        </p>
    </div>
  )
}

export default Motivation

--- File Index 12: src/pages/home.js ---
import React, { useState } from 'react'
import {  Form, Button } from 'react-bootstrap'
import {useNavigate} from 'react-router-dom'
import ShortUniqueId from 'short-unique-id';
import toast from 'react-hot-toast'
import copy from 'copy-to-clipboard';
import { Carousel } from 'react-bootstrap';
import {InfoCircleFill} from 'react-bootstrap-icons'
import Footer from '../components/footer';

const Home = () => {
    const Suid = new ShortUniqueId()
    const history = useNavigate()
    const valt = 'Enter the Session Id', invalt = 'Invalid Session  Id';
    const lenvalt = 'Enter only 10 characters Session Id';
    const [uname, setuname] = useState('')    
    const [fclick, setFclick] = useState(false)
    const [sid, setSid] = useState('')
    const [sid1, setSid1] = useState('')
    const [validText, setvalidText] = useState(valt)
    const alphanumericRegex = /^[a-zA-Z0-9]+$/;
    const [interviewer, setInterviewer] = useState(false)
  

    const handleChange1 = (e) => {
        const input = e.target.value
        if (input && !alphanumericRegex.test(input)) {
            setvalidText(invalt)
            setSid1('')
            return;
        } 
        setvalidText(lenvalt)
        if(input.length>10) {
            setSid1('')
            return
        }
        setSid1(input)
    }

    const handleNameChange = (e) => {
        setuname(e.target.value)
    }

    const generateCode = (e) => {
        e.preventDefault();
        setFclick(true)
        let code  = Suid(10);
        setSid(code)
        setSid1(code)
        if (copy(code))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    const handleSwitch = (e) => {
        setInterviewer(!interviewer)
        // console.log(interviewer);
    }

    const handleJoin = async (e)=> {
        e.preventDefault()
        if(sid.length===0 && sid1.length===0) {
            toast('Please generate or fill the Session ID', {
                icon: '❕',
              });
            return;
        }
        if(uname.length==0) {
            toast.error("Name field is empty")
            return;
        }
        const roomId = sid.length>0?sid:sid1
        if(sid.length>0) {
            localStorage.setItem('init', 'true')
        }
        toast.success('Joining new Call')
        history(`/call/${roomId}`, {
            state: {
                username: uname,
                interviewer:!interviewer,
            },
        });
    }
    
    return (
        <>
        <div className='main-home'>
            <div className='cen-container'>
                {/* <Image src='/qna.png' className='img-fluid' /> */}
                <div className='lcentral l-shadow'>
                <Carousel data-bs-theme="dark" className='fwid'>
                    <Carousel.Item key='1' interval={500}>
                        <img
                        className="d-block w-100 img-fluid"
                        src="/qna.png"
                        alt="First slide"
                        />
                        <Carousel.Caption>
                        <h5 className='blk-txt'>Online Interviews</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='2' interval={750}>
                        <img alt='face motion' className="d-block w-100 img-fluid" src='/face scanner.png'/>
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Face Motion Detection</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='3' interval={750}>
                        <img alt='video' className="d-block w-100 img-fluid"  src="/interview.jpg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Video Interview</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='4' interval={750}>
                        <img alt='code editor' className="d-block w-100 img-fluid"  src="/editor.jpeg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Live Code Editor</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='5' interval={750}>
                        <img alt='live chat' className="d-block w-100 img-fluid"  src="/chat.png" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'> Live Chat</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    
                </Carousel>
                </div>
            </div>
            <div className='cen-container'>
                <div className='central r-shadow'>
                    <div className='d-flex flex-column'>
                        <div className='jcontent'>
                            <h3 className='t-color'>Welcome to <span className='myfont'>CodeCam</span></h3>

                        </div>
                        <div className='jcontent mt4'>
                            <h4>Start a New Session</h4>
                            {/* <Form> */}
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    
                                    <Form.Control 
                                        className='genInp' 
                                        type="text" 
                                        placeholder="#uniqueId" 
                                        disabled 
                                        name="sid"
                                        value={sid}
                                    />
                                        <Button className="btn-dark gen-btn"
                                            onClick={generateCode}
                                        >Generate {fclick ? 'Again' : ''}</Button>
                                </Form.Group>
                                <h4>or</h4>
                            
                        </div>
                        <div className='jcontent mt-4'>
                            <h4>Join with Code</h4>
                                <Form.Group className="mmb fin " >
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Id" 
                                        name="sid1"
                                        onChange={handleChange1}
                                        value={sid1}
                                        
                                    />
                                    <Form.Text>{validText}</Form.Text>
                                </Form.Group>
                        </div>
                        <div className='jcontent'>
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Enter your good Name" 
                       
                                        name="uname"
                                        onChange={handleNameChange}
                                        value={uname}
                                    />
                                </Form.Group>
                                <Form.Group className='ml-2'>
                                    <Form.Check
                                        type="switch"
                                        id="custom-switch"
                                        label="Join as Interviewer "
                                        value={interviewer}
                                        name='interviewer'
                                        onChange={handleSwitch}
                                        style={{display: 'inline', textAlign: 'left !important', padding: '5px'}}
                                    />
                                    <InfoCircleFill className='infoIcon'></InfoCircleFill>
                                    <span className="afterHoverText">Toggle to disable Face Monitoring</span>
                                </Form.Group>
                                <Form.Group className='mt-2'>
                                    <Button onClick={handleJoin}>Join Call</Button>
                                </Form.Group>

                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div className='disclaimer'>
            <h5>Note:</h5>
            <p>Sit in well lit room and Face camera to avoid misbehaviour notifications. <br></br>
            If your video is not visible, hard reload the page and rejoin.
            </p>
        </div>
        </>
    )
}

export default Home

--- File Index 13: src/pages/peerCall.js ---
import React, { useState, createContext, useRef, useEffect } from "react";

import toast from 'react-hot-toast';
import { useLocation, useNavigate, useParams, Navigate } from 'react-router-dom';
import { Peer } from 'peerjs'
import { Button } from "react-bootstrap";
import * as faceapi from 'face-api.js'
import Chat from "../components/pchat";
import copy from 'copy-to-clipboard';
import Editor from "../components/peditor";
import "../styles/callpage.css";
const server = process.env.REACT_APP_BACKEND_URI;
const server_host = process.env.REACT_APP_BACKEND_HOST;
const PeerCall = () => {
    const location = useLocation();
    const history = useNavigate();
    const { roomId } = useParams();

    const myName = location.state?.username
    const interviewer = location.state?.interviewer

    const MOTION_THRESHOLD = 40;
    // const myConns = new Map()
    const [myConns, setMyConns] = useState(new Map())
    const [mycalls, setMyCalls] = useState(new Set())
    const idName = new Map()


    var userMoves = 0;
    var previousLandmarks = null;
    var warned = false;
    var peer = null
    var dataStream = null
    var screenStream = null
    var myPeerId;
    var timer;

    const [allPeers, setAllPeers] = useState([])
    const [stream, setstream] = useState(null)

    const [lang, setLang] = useState('javascript')
    const [chatState, setChatState] = useState(false)
    const [displayCheck, setDisplayCheck] = useState(false)
    const [sirId, setsirId] = useState(null)
    const myVideo = useRef()
    const [code, setcode] = useState(`console.log("Live shared Code Editor")`)

    //Face motion logic
    function analyzeFaceMotions(landmarks) {
        const currentLandmarks = landmarks._positions;

        if (previousLandmarks && currentLandmarks.length == 68) {
            let totalMotion = 0, averageMotion = 0;
            for (let i = 0; i < 68; i++) {
                const dx = currentLandmarks[i]._x - previousLandmarks[i]._x;
                const dy = currentLandmarks[i]._y - previousLandmarks[i]._y;
                const distance = Math.sqrt(dx * dx + dy * dy)
                totalMotion += distance
            }
            averageMotion = totalMotion / 68;
            // console.log(averageMotion);
            if (averageMotion > MOTION_THRESHOLD) {
                toast('Face motion detected, Please concentrate on the interview', {
                    icon: '❕',
                });
                userMoves++;
            }
        }
        previousLandmarks = currentLandmarks;
    }

    async function detectFaceMotions() {
        if (!myVideo.current) return;

        timer = setInterval(async () => {
            if (warned) {
                if (sirId) { }
                // socketRef?.current?.emit(ACTIONS.BEHAVIOUR , {roomId})
                else {
                    toast.error('Please Behave until the Interviewer joins')
                }
                warned = false
                userMoves = 0
                return
            }
            if (userMoves > 12) {
                toast.error("Warning! Interviewer will be notified if movement is observed again.")
                userMoves = userMoves - 2;
                warned = true;
            }
            const detections = await faceapi.detectAllFaces(myVideo.current,
                new faceapi.TinyFaceDetectorOptions())
                .withFaceLandmarks()

            if (myVideo.current && detections.length == 0) {
                toast.error("Please sit in a well lit room and face the Webcam!")
                alert('Face cannot be detected ! Please sit in a weel lit area.')
                userMoves++;
            }
            else if (detections?.length > 1) {
                toast.error(`${detections.length} persons spotted in camera`)
                userMoves++;
            }
            else if (detections && detections.length == 1) {
                analyzeFaceMotions(detections[0].landmarks)
            } else {
                console.log(detections);
                toast.error(" Please face the webcam!")
            }

        }, 2500)
    }

    useEffect(() => {
        //take camera permission
        navigator?.mediaDevices?.getUserMedia({ video: true, audio: true })
            .then(videoStream => {
                dataStream = videoStream;
                setstream(videoStream);

                // initialize socket
                init(videoStream)

            })
            .catch((error) => {
                console.log(error);
                toast.error('Cannot connect without video and audio permissions')
                return <Navigate to="/" />;
            });
        //load faceapi Models
        loadModels()
        //socket connecting function
        const init = async (videoStream) => {
            // PeerJS functionality starts 
            var peer_params = {
                host: server_host,
                path: '/myapp',
            }
            if (server_host == 'localhost') peer_params['port'] = 3007
            peer = new Peer(peer_params)
            // once peer created join room
            peer.on('open', function (id) {
                myPeerId = id

                addVideo(videoStream, id, myName)
                if (interviewer)
                    detectFaceMotions()
                let flag = interviewer ? false : true
                fetch(`${server}join`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        username: myName,
                        peerId: id,
                        flag
                    })
                }).then(res => res.json())
                    .then(res_arr => {
                        const { data } = res_arr
                        setAllPeers(data);
                        for (const { peerId, username } of data) {

                            let conn = peer.connect(peerId)

                            conn.on("open", () => {
                                myConns.set(conn, username)
                                conn.send({ sig: 1, data: { id, name: myName } })
                                let call = peer.call(peerId, videoStream)
                                call.on('stream', (remoteStream) => {
                                    mycalls.add(call)
                                    addVideo(remoteStream, peerId, username)
                                })
                            })

                            conn.on('data', ({ sig, data }) => {
                                recvHandler(conn, sig, data)
                            })

                            conn.on('close', () => {
                                myConns.delete(conn)
                                const element = document.getElementById(peerId);
                                element?.remove();
                            })

                            if (peerId == sirId) {
                                toast.success('Interviewer Joined')
                            }
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    })

            });

            peer.on("connection", (conn) => {
                conn.on('data', ({ sig, data }) => {
                    recvHandler(conn, sig, data)
                })
                conn.on('close', () => {
                    myConns.delete(conn)
                    toast.success(`${idName[conn.peer]} left the Call`)
                    const element = document.getElementById(conn.peer);
                    element?.remove();
                })
            });

            peer.on('call', (call) => {
                call.answer(videoStream)
                mycalls.add(call)
                call.on('stream', (remoteStream) => {
                    addVideo(remoteStream, call.peer, idName[call.peer])
                })
                toast.success(`${idName[call.peer]} joined the Call`)
            })
        };

        // Clean up function to remove camera permissions and end socket
        return () => {
            const ele = document.getElementById(myPeerId)
            ele?.remove()
            clearInterval(timer)
            if (peer) {
                try {
                    fetch(`${server}leave`, {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json"
                        },
                        body: JSON.stringify({
                            roomId,
                            peerId: myPeerId,
                        })
                    })
                } catch (err) {
                    console.log(err);
                    toast.error("Coudn't leave the room at the current moment")
                }
                peer.destroy();
            }
            if (dataStream) {
                const tracks = dataStream.getTracks();
                tracks.forEach((track) => track.stop());
            }
        };
    }, []);

    //Funtions to be triggered by child components passed as props

    const sendHandler = (sig, data) => {
        for (const [conn, name] of myConns) {
            conn.send({ sig, data })
        }
    }

    const recvHandler = (conn, sig, data) => {
        switch (sig) {
            case 1: {
                //share name
                const { id, name } = data
                myConns.set(conn, name)
                idName[id] = name
                conn.send(3, { value: code })
                break;
            }
            case 2: {
                // chat msg
                const { username, msg } = data
                addRecvMsg(msg, username)
            }
            case 3: {
                // code update
                const { value } = data
                console.log(value);
                if (value != code)
                    setcode(value);
            }
            case 4: {
                // language change
                const { newLang } = data
                if (lang != newLang) setLang(newLang)
            }
            default:
                break;
        }
    }

    const addRecvMsg = (text, name = 'Unknown') => {
        setChatState(true)
        const element =
            ` <div class='receive'>
                    <div class='msg'>
                    <span class='senderName' >${name}</span>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML = element
        rdiv.setAttribute('class', 'msg-container')
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)
        if (par)
            par.scrollTop = par.scrollHeight
    }


    //Functions related to this component
    function addVideo(vstream, peerID, userName = "user") {
        const prev = document.getElementById(peerID)
        prev?.remove()
        const row = document.createElement('div')
        row.setAttribute('class', 'mt-2')
        row.setAttribute('id', peerID)

        const video = document.createElement('video')
        video.srcObject = vstream;
        video.addEventListener('loadedmetadata', () => {
            video.play()
        })
        // console.log(peerID, myPeerId);
        if (peerID == myPeerId) {
            video.muted = true
            myVideo.current = video
        }

        const span = document.createElement('span')
        span.innerText = userName
        span.setAttribute('class', 'tagName')

        row.append(video)
        row.append(span)

        const exist = document.getElementById(peerID)
        if (exist) return

        const peerDiv = document.getElementById('peerDiv')

        peerDiv?.insertBefore(row, peerDiv.children[0])

    }

    const loadModels = () => {
        Promise.all([
            faceapi.nets.faceLandmark68Net.loadFromUri("/models"),
            faceapi.nets.tinyFaceDetector.loadFromUri("/models"),
            faceapi.loadFaceLandmarkTinyModel("/models")
        ]).then(() => {
            // console.log('Models Loaded');
        }).catch(err => {
            console.log('FaceAPI modules loading error', err);
        })

    }

    const screenShareHandler = async (e) => {
        e.preventDefault()
        setDisplayCheck(!displayCheck)
        idName['screen'] = 'screen'
        navigator?.mediaDevices?.getDisplayMedia({ audio: true }).then(displStream => {
            screenStream = displStream
            addVideo(displStream, 'screen', 'Screen')
            replaceStream(displStream)
        }).catch((error) => {
            console.log(error);
        });

    }

    const replaceStream = (mediaStream) => {
        for (const call of mycalls) {
            for (let sender of call.peerConnection?.getSenders()) {
                if (sender.track.kind == "audio") {
                    if (mediaStream.getAudioTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getAudioTracks()[0]);
                    }
                }
                if (sender.track.kind == "video") {
                    if (mediaStream.getVideoTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getVideoTracks()[0]);
                    }
                }
            }
        }
    }


    function stopCapture(evt) {
        evt.preventDefault()
        setDisplayCheck(!displayCheck)
        replaceStream(stream)
        let ele = document.getElementById('screen')
        const evid = ele?.childNodes[1];
        if (evid && evid.srcObject) {
            const tracks = evid.srcObject.getTracks();
            tracks.forEach((track) => track.stop());
        }
        ele?.remove();
    }
    const copyCode = (e) => {
        e.preventDefault();
        if (copy(roomId))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    function chatHider() {
        setChatState(chatState => !chatState)
    }

    function leaveRoom() {
        if (peer) {
            try {
                fetch(`${server}leave`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        peerId: myPeerId,
                    })
                })
            } catch (err) {
                console.log(err);
                toast.error("Coudn't leave the room at the current moment")
            }
            peer.destroy()
        }
        if (dataStream) {
            const tracks = dataStream.getTracks();
            tracks.forEach((track) => track.stop());
        }
        clearInterval(timer)
        history('/');
    }

    if (!location.state) {
        return <Navigate to="/" />;
    }


    return (
        <>
            <div style={{ marginBottom: "50px", display: "flex", flexDirection: "row", justifyContent: "space-between" }} >
                <div style={{ marginTop: "20px" }}>
                    <div className='econt'>
                        <Editor
                            conns={myConns}
                            roomId={roomId}
                            onCodeChange={setcode}
                            code={code}
                            lang={lang}
                            peer={peer}
                            sendHandler={sendHandler}
                        />
                    </div>
                </div>
                <div style={{
                    display: "flex",
                    flexDirection: "column",
                    alignItems: "center",
                    width: "100%",
                    marginTop: "20px"
                }}>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        gap: "20px",
                        width: "100%",
                    }}>
                        <Button
                            onClick={leaveRoom}
                            className="mt-2 btn-danger"
                            style={{ width: '120px', margin: 'auto' }}>Leave</Button>
                        <Button
                            onClick={!displayCheck ? screenShareHandler : stopCapture} className="mt-2" style={{ width: '170px', margin: 'auto' }}>{!displayCheck ? 'Screen Share' : 'Stop Share'}</Button>
                        <Button
                            onClick={copyCode}
                            className="mt-2 btn-info"
                            style={{ width: '160px', margin: 'auto' }}>
                            Copy Session Id
                        </Button>
                        <div onClick={chatHider}>
                            {!chatState ?
                                <img style={{ width: "50px" }} src="/toggle_chat.png"></img>
                                : <></>
                            }
                        </div>
                    </div>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        marginTop: "100px",
                        width: "100%",
                        justifyContent: "space-evenly"
                    }} id="peerDiv">
                        {/* <div style={{ margin: "0px 10px" }} className='' id="users">
                        </div> */}
                    </div>
                </div>

            </div >

            {!chatState ?
                <></>
                : <>
                    <Chat
                        recvMsg={addRecvMsg}
                        conns={myConns}
                        roomId={roomId}
                        username={myName}
                        sendHandler={sendHandler}
                        peer={peer}
                        className='chatCont'
                    />
                    <img src="/close_chat.png" onClick={chatHider} className="fix_close"></img>
                </>
            }
        </>
    )
}

export default PeerCall

--- File Index 14: src/pages/addSlides.js ---
import React, {useState} from 'react'
import { Form , Button} from 'react-bootstrap'
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const AddSlides = () => {

    const [heading, setHeading] = useState("");
    const [body, setBody] = useState("");

    const handleSubmit = async (e) => {
        e.preventDefault();

        const data = {
        heading: heading,
        body: body
        };

        try {
            const response = await fetch(`${serverLink}/save_data`, {
                method: "POST",
                headers: {
                "Content-Type": "application/json"
                },
                body: JSON.stringify(data)
            });
          
            if (response.ok) {
            //   console.log("Data saved successfully");
                toast.success("Slides Updated")
            } else {
              console.error("Error saving data:", response.status);
              toast.error('Error updating')
            }
          } catch (error) {
            console.error("Error saving data:", error);
            toast.error("Error sending data")
          }
          

        // Reset form fields
        setHeading("");
        setBody("");
    };



    return (
        <div className='cen-container mt-5 lcentral l-shadow'>
            <h3>Add New Slides</h3>
            <form onSubmit={handleSubmit} >
                {/* <label htmlFor="heading">Heading:</label> */}
                {/* <input
                    type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required
                /> */}
                
                {/* <label htmlFor="body">Body:</label>
                <textarea
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                ></textarea> */}
                <Form.Group className="mb-3" >
                    <Form.Label>Heading:</Form.Label>
                    <Form.Control type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required />
                </Form.Group>
                <Form.Group className="mb-3" >
                    <Form.Label>Body:</Form.Label>
                    <Form.Control as="textarea" 
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                    rows={3} />
                </Form.Group>

                <Button type="submit">Submit</Button>
            </form>
        </div>
    )
}

export default AddSlides

--- File Index 15: src/pages/slides.js ---
import React, {useState, useEffect} from 'react'
import { Carousel } from 'react-bootstrap';
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const Slides = () => {
    let local_Data = localStorage.getItem('slides')
    const [data, setData] = useState(local_Data!=undefined ? JSON.parse(local_Data) : [])

    useEffect(() => {
        async function fetchData() {
            try {
                const res= await fetch(`${serverLink}get_data`, {
                    method: "GET",
                    headers: {
                    "Content-Type": "application/json"
                    },
                });
                const newData = await res.json();
                setData(newData)
                localStorage.setItem('slides', JSON.stringify(newData))
            } catch(error) {
                console.log(error);
                toast.error("Error receiving data")
            }
        }
        fetchData();
        
    }, [])

    return (
        <div className=''>
            <div className='cen-container l-shadow lcentral mt-5 '>
                <Carousel>
                {   
                    data.map((item, index) => (
                        
                        <Carousel.Item key={index} interval={500}>
                            <div key={index} className='f-height'>
                                <h2>{item.heading}</h2>
                                <h6>{item.body }</h6>
                            </div>
                        </Carousel.Item>

                ))}
                </Carousel>
            </div>
        </div>
    )
}

export default Slides

--- File Index 16: src/reducers/userReducer.js ---
export const initialState = null;

export const reducer = (state, action) => {
	if (action.type === "USER") {
		return action.payload;
	}if (action.type === "CLEAR") {
		return null;
	}
	return state;
};



Analyze the codebase context.
Identify the top 5-10 core most important abstractions to help those new to the codebase.

For each abstraction, provide:
1. A concise `name`.
2. A beginner-friendly `description` explaining what it is with a simple analogy, in around 100 words.
3. A list of relevant `file_indices` (integers) using the format `idx # path/comment`.

List of file indices and paths present in the context:
- 0 # README.md
- 1 # src/Actions.js
- 2 # src/index.js
- 3 # src/socket.js
- 4 # src/App.js
- 5 # src/components/navbar.js
- 6 # src/components/peditor.js
- 7 # src/components/footer.js
- 8 # src/components/pchat.js
- 9 # src/pages/NotFound.js
- 10 # src/pages/features.js
- 11 # src/pages/motivation.js
- 12 # src/pages/home.js
- 13 # src/pages/peerCall.js
- 14 # src/pages/addSlides.js
- 15 # src/pages/slides.js
- 16 # src/reducers/userReducer.js

Format the output as a YAML list of dictionaries:

```yaml
- name: |
    Query Processing
  description: |
    Explains what the abstraction does.
    It's like a central dispatcher routing requests.
  file_indices:
    - 0 # path/to/file1.py
    - 3 # path/to/related.py
- name: |
    Query Optimization
  description: |
    Another core concept, similar to a blueprint for objects.
  file_indices:
    - 5 # path/to/another.js
# ... up to 10 abstractions
```
2025-05-16 17:51:05,942 - INFO - PROMPT: 
For the project `CollabCam`:

Codebase Context:
--- File Index 0: README.md ---
# Welcome to CollabCam

**CollabCam** is a dynamic web platform specifically built to facilitate **online coding interviews**. Designed to streamline the interview process, CollabCam allows interviewers to engage with candidates in real-time through a collaborative environment that integrates coding, monitoring, and communication tools.

With CollabCam, interviewers can initiate a new session, share the unique session code with candidates, and dive into an interactive interview experience that mirrors the dynamics of an in-person coding test.

## Key Features

- ### **Collaborative Code Editor**
  CollabCam features a **live coding editor** where candidates can write, edit, and debug code directly in the interview session. The shared editor updates in real-time, allowing interviewers to observe the candidate’s approach, provide feedback, and guide them through technical questions or coding challenges seamlessly.

- ### **Video Monitoring**
  To ensure integrity and focus, CollabCam includes a **video monitoring tool** that tracks head movements of the candidate. When a candidate turns on their video, CollabCam will monitor and detect excessive head movements:
  - If notable movement is being detected, CollabCam issues a **warning**.
  - Continued movement post-warning may result in the **session being terminated**, allowing interviewers to maintain a controlled environment.

- ### **In-Session Chat**
  CollabCam also provides a built-in **chat feature** for smooth communication between the interviewer and candidate. Whether discussing the task at hand, providing hints, or addressing technical issues, the chat function ensures clear and effective interaction throughout the session.

> **Note**: Currently, CollabCam’s functionality is optimized for users connected on the same network due to limitations with the current PeerJS implementation. The development roadmap includes plans to expand this capability, enabling users to connect from any network globally.

## Future Scope

We have exciting plans for future updates that will enhance CollabCam’s utility:
- **Session Management**: Enable interviewers to conduct consecutive interviews without needing to reset the platform, making back-to-back interviews seamless.
- **Scoring and Feedback**: Add functionality for interviewers to assign scores, write feedback, and maintain a log of candidate performance over multiple sessions.

## Get Started

You can try out CollabCam and explore its features by visiting the link below:

[Access CollabCam](https://collabcam.onrender.com/)

---

With CollabCam, conducting remote technical interviews is simpler, more efficient, and fully interactive, providing a comprehensive environment for both interviewers and candidates.


--- File Index 1: src/Actions.js ---
const ACTIONS = {
    JOIN: 'join',
    JOINED: 'joined',
    DISCONNECTED: 'disconnected',
    CODE_CHANGE: 'code-change',
    SYNC_CODE: 'sync-code',
    SHARE_PEER_IDS: 'share-peer-ids',
    SIR_JOined: 'interviewer-joined',
    MONITOR: 'monitor',
    BEHAVIOUR: 'mis-behaving',
    SEND_MSG : 'send-message',
    RECV_MSG : 'receive-message',
    LANG_CHANGE: 'change-language',
    UPDATE_LAN: 'update-language',
    LEAVE: 'leave',
};

module.exports = ACTIONS;

--- File Index 2: src/index.js ---
import React from 'react';
import {createRoot} from 'react-dom/client'
import './index.css';
import App from './App';
// import { ContextProvider } from './Context';
import 'bootstrap/dist/css/bootstrap.min.css';

const root_element = document.getElementById('root');
const root= createRoot(root_element)
root.render(<App />);

// root.render(
//     <React.StrictMode>
//         {/* <ContextProvider> */}
//             <App/>
//         {/* </ContextProvider> */}
//     </React.StrictMode>
// );


--- File Index 3: src/socket.js ---
import { io } from 'socket.io-client';

export const initSocket = async () => {
    const options = {
        'force new connection': true,
        reconnectionAttempt: 'Infinity',
        timeout: 10000,
        transports: ['websocket'],
    };
    // console.log('conecting to socket');
    // return io('https://c2f-api-vineethkumarm.koyeb.app:8000', options);
    return io('https://CodeCamapi.onrender.com', options);
    // return io('https://amazing-croissant-5bbe89.netlify.app/', options);
    // return io('https://c2f-api.el.r.appspot.com', options);
    // return io('http://localhost:3007',options);
    // return io()
    console.log(process.env);
    return io(process.env.REACT_APP_BACKEND_URI, options);
};

--- File Index 4: src/App.js ---
import './App.css';
import React,{createContext, useReducer} from 'react';
import {Route , BrowserRouter as Router,Routes} from 'react-router-dom'
import { initialState, reducer } from './reducers/userReducer';
import 'react-bootstrap'
//components and pages
import MyNavbar from './components/navbar';
import Home from './pages/home';
import NotFound from './pages/NotFound';
// import Testing from './pages/testing';
import { Toaster } from 'react-hot-toast';
import Slides from './pages/slides';
import AddSlides from './pages/addSlides';
import PeerCall from './pages/peerCall';
import Motivation from './pages/motivation';
import Features from './pages/features';
import Footer from './components/footer';

export const UserContext = createContext();

function App() {


	const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <UserContext.Provider value={{state,dispatch}}>
            <div>
                <Toaster 
                    position='top-right'
                />
            </div>
            <Router >
                <div className='wrapper' >

                <div className='App'>
                    <MyNavbar/>
                    <div className='content d-flex'>
                        <Routes>
                                <Route eaxct path="/" element={<Home/>} />
                                {/* <Route exact path="/call/:roomId" element={<CallPage/>} /> */}
                                <Route exact path="/call/:roomId" element={<PeerCall/>} />
                                <Route exact path='/slides' element={<Slides/>} />
                                <Route exact path='/add' element={<AddSlides/>} />
                                {/* <Route path="/chat" element={<Chat/>} /> */}
                                <Route exact path='/motivation' element={<Motivation />} />
                                {/* <Route exact path='/features' element={<Features />} /> */}
                                <Route path="*" element={<NotFound/>} />
                        </Routes>
                    </div>
                </div>
                    {/* <Footer /> */}
                </div>
            </Router>
         </UserContext.Provider>
		
	
	);
}

export default App;


--- File Index 5: src/components/navbar.js ---
import React,{ useContext } from "react";
import { Link } from "react-router-dom";
import { UserContext } from "../App";
import { useNavigate } from "react-router-dom";
import { NavbarBrand } from "react-bootstrap";
import "../styles/navbar.css"

const Navbar = () => {
	let { state, dispatch } = useContext(UserContext);
	const history = useNavigate()
	let jwt = localStorage.getItem("jwt")
	if(jwt) {
		state=true;
	}


	const renderList = () => {
		if (state) {
			return [
				<>
					<Link key='profile' className="navlink" to="/profile">Profile</Link>
					<Link key='dsf' className="navlink" to="/newPost">new post</Link>
					<button key={jwt.slice(0,3)} className="btn btn-primary" onClick={() => {
							localStorage.clear();
							dispatch({type:"CLEAR"})
							history('/')
						}
						}>
						Logout
					</button>
				
				</>
			];
		} else {
			return [
				<>
                    {/* <Link to='/testing' className="navlink">Test</Link>  */}
					{/* <Link key='login' className="navlink" to="/login">Login</Link> */}
					{/* <Link key='register' className="navlink" to="/register">Register</Link> */}
					<Link key='slides' className="navlink" to="/motivation">Motivation</Link>
					{/* <Link key='slides' className="navlink" to="/features">Features</Link> */}
					<Link key='slides' className="navlink" to="/slides">Revision</Link>
					
				</>,
			];
		}
	};

	return (
		<nav className="main-nav">
			{/* <div className="logo" ><h3 href="/">CodeCam</h3></div> */}
            <NavbarBrand className="logo">
                <Link key='logo' to="/">
                    CodeCam
                    {/* Test */}
                </Link>
            </NavbarBrand>
			<div className="navlinks">{renderList()}</div>
		</nav>
	);
};

export default Navbar;

--- File Index 6: src/components/peditor.js ---
import React, { useEffect, useState, useRef, useMemo } from 'react'
import { useLocation } from 'react-router-dom';
import { Button, Form } from 'react-bootstrap';
// codemirror components
import { useCodeMirror } from '@uiw/react-codemirror';
import Select from 'react-select';

// import languages 
import { javascript } from '@codemirror/lang-javascript';
import { cpp } from '@codemirror/lang-cpp';
import { python } from '@codemirror/lang-python';
import { java } from '@codemirror/lang-java';

// import themes 
import { githubDark } from '@uiw/codemirror-theme-github';
import { xcodeDark, xcodeLight } from '@uiw/codemirror-theme-xcode';
import { eclipse } from '@uiw/codemirror-theme-eclipse';
import { abcdef } from '@uiw/codemirror-theme-abcdef';
import { solarizedDark } from '@uiw/codemirror-theme-solarized';
import "../styles/editor.css"
import { toast } from 'react-hot-toast';

var qs = require('qs');


// const extensions = [javascript()]


const Editor = ({ sendHandler, roomId, onCodeChange, code, lang }) => {
    const history = useLocation()
    const location = useLocation()
    const uname = location?.state?.username
    const [theme, setTheme] = useState(githubDark);
    // const [code, se==1ode] = useState(code);
    const [selectValue, setSelectValue] = useState('javascript')
    const [extensions, setExtensions] = useState([javascript()])
    const [placeholder, setPlaceholder] = useState('Please enter the code.');
    const [input, setInput] = useState('')
    const [output, setOutput] = useState('')
    const [ran, setran] = useState(false)
    const [tc, setTc] = useState(true)
    const thememap = new Map()
    const langnMap = new Map()

    const editorRef = useRef(null)
    const { setContainer } = useCodeMirror({
        container: editorRef.current,
        extensions,
        value: code,
        theme,
        editable: true,
        height: `70vh`,
        width: `45vw`,
        basicSetup: {
            foldGutter: false,
            dropCursor: false,
            indentOnInput: false,
        },
        options: {
            autoComplete: true,
        },
        placeholder: placeholder,
        style: {
            position: `relative`,
            zIndex: `999`,
            borderRadius: `10px`,
        },
        onChange: (value) => {
            onCodeChange(value)
            sendHandler(3, { value })
        }

    })


    const themeInit = () => {
        thememap.set("githubDark", githubDark)
        thememap.set("xcodeDark", xcodeDark)
        thememap.set("eclipse", eclipse)
        thememap.set("xcodeLight", xcodeLight)
        thememap.set("abcdef", abcdef)
        thememap.set("solarizedDark", solarizedDark)
    }

    const langInit = () => {
        langnMap.set('java', java)
        langnMap.set('cpp', cpp)
        langnMap.set("javascript", javascript)
        langnMap.set('python', python)
    }


    const handleThemeChange = (event) => {
        setTheme(thememap.get(event.value));
    };

    const handleLanguageChange = (event) => {
        setExtensions([langnMap.get(event.value)()]);

        sendHandler(4, { newLang: event.value })
        setSelectValue(event.value)
    };

    themeInit()
    langInit()

    function langCode(e) {
        if (e == 'javascript') return 'js';
        else if (e == 'python') return 'py';
        return e;
    }

    useEffect(() => {
        if (!editorRef.current) {
            alert('error loading editor')
            history('/')
        }
        if (editorRef.current) {
            setContainer(editorRef.current)
        }

    }, [editorRef.current])

    useEffect(() => {
        if (lang != selectValue && lang) {
            setSelectValue(lang)
            setExtensions([langnMap.get(lang)()]);
        }
    }, [lang])

    const compile = (e) => {
        e.preventDefault();
        var data = qs.stringify({
            'code': code,
            'language': langCode(selectValue),
            'input': input
        });
        var config = {
            method: 'post',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            },
            body: data
        };
        fetch('https://api.codex.jaagrav.in', config)
            .then(res => res.json())
            .then(data => {
                if (data['error'].length == 0) {
                    setTc(true)
                    toast.success("compiled sucessfully")
                    setOutput(data['output'])
                }
                else {
                    setTc(false)
                    toast.error("compilation error")
                    setOutput(data['error'])
                }
            })
            .catch(function (error) {
                console.log(error);
            });
    }

    const handleInputChange = (e) => {
        setInput(e.target.value)
    }
    const handleOutputChange = (e) => {
        setOutput(e.target.value)
    }

    const themeOptions = [
        { value: "githubDark", label: "Github Dark" },
        { value: "eclipse", label: "Eclipse" },
        { value: "xcodelight", label: "X Code Light" },
        { value: "xcodedark", label: "X Code Dark" },
        { value: "solarizeddark", label: "Solarized Dark" },
        { value: "abcdef", label: "ABCDEFs" },
    ]

    const langOptions = [
        { value: "javascript", label: "JavaScript" },
        { value: "java", label: "Java" },
        { value: "cpp", label: "C++" },
        { value: "python", label: "Python" }
    ]

    return (
        <div className='editorcomponent'>
            <div style={{
                display: "flex",
                gap: "10px",
                flexDirection: "column"
            }}>
                <div>
                    <span>Theme:</span>
                    <Select
                        onChange={handleThemeChange}
                        classNamePrefix="select"
                        defaultValue={themeOptions[0]}
                        name="color"
                        options={themeOptions}
                    />
                </div>
                <div>
                    <span>Language:</span>
                    <Select
                        onChange={handleLanguageChange}
                        className="basic-single"
                        classNamePrefix="select"
                        defaultValue={langOptions[0]}
                        name="color"
                        options={langOptions}
                    />
                </div>
                <button className='run ' onClick={compile} >Run</button>
            </div>
            <div style={{marginTop: "20px"}} ref={editorRef} className='ide' ></div>
            <div style={{display: "flex", flexDirection: "row", justifyContent: "space-between", marginTop: "20px"}} className='iodiv d-flex flex-wrap'>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Input</Form.Label>
                    <Form.Control as="textarea" rows={2} value={input} onChange={handleInputChange} />
                </Form.Group>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Output</Form.Label>
                    <Form.Control as="textarea" rows={2} value={output} className={tc ? 'ctxt' : 'etxt'} onChange={handleOutputChange} disabled />
                </Form.Group>

            </div>
        </div>
    );
}

export default Editor

--- File Index 7: src/components/footer.js ---
import React from 'react'
import "../styles/footer.css"
const Footer = () => {
  return (
    <footer className="page-footer font-small bg-dark text-center text-white pt-2">

        <div className="footer-copyright text-center py-3">
            <h5>Made With ❤️ by Aaditya Kumar </h5>
        </div>

    </footer>
    )
}

export default Footer


--- File Index 8: src/components/pchat.js ---
import React, {useEffect, useState} from 'react'
import { Form } from 'react-bootstrap'

import "../styles/chat.css"

const Chat = ({sendHandler, roomId, username}) => {

    const [msg, setmsg] = useState('')

    const handleChange = (e) => {
        setmsg(e.target.value)
    }
    const handleSend = (e) => {
        e.preventDefault();
        if(msg.length==0) return;
        sendHandler(2,{username, msg})
        addSendMsg(msg)
        setmsg('')
    }
    
    const addSendMsg = (text) => {
        const element = 
            ` <div class='send'>
                    <div class='msg'>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML=element
        rdiv.setAttribute('class', 'msg-container')
        
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)  
        par.scrollTop = par.scrollHeight  
    }


    return (
        <div className='chatCont mt5'>
            <div className='chatBox'>
                <div className='scroller' id='msg-div'>
                </div>
                <div className='minp'>
                    <Form onSubmit={handleSend}>

                    
                        <Form.Control
                            type="text" 
                            placeholder="Enter message" 
                            name="msg"
                            onChange={handleChange}
                            value={msg}
                            className='finp inlin'
                        />
                        {/* <Button className='inlin' onClick={handleSend}> Send</Button> */}
                    </Form>
                </div>
            </div>
        </div>
    )
}

export default Chat

--- File Index 9: src/pages/NotFound.js ---
import React from 'react'

const NotFound = () => {
  return (
    <div className='errorpage'><span>Sorry, the request resources are not available on this website</span></div>
  )
}

export default NotFound

--- File Index 10: src/pages/features.js ---
import React from 'react'

const Features = () => {
  return (
    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Features</h2>
        <ul>
            <li>Video Chatting</li>
            <li>Screen Sharing</li>
            <li>Face Monitoring</li>
            <li>Collaborative Code Editor</li>
            <li>Online Compiler</li>
        </ul>
    </div>
  )
}

export default Features

--- File Index 11: src/pages/motivation.js ---
import React from 'react'

const Motivation = () => {
  return (

    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Motivation</h2>
        <p>Introducing the ultimate solution for seamless and efficient online interviews - this innovative 
            project brings together the power of video chat, code editor, and compiler all in one platform!
             No more juggling between multiple tabs or applications during interviews. With this website, you 
             can now focus solely on showcasing your skills and acing those coding challenges. Embrace the 
             ease of navigating through the interview process effortlessly, while impressing your potential 
             employers with your coding prowess.

        </p>
    </div>
  )
}

export default Motivation

--- File Index 12: src/pages/home.js ---
import React, { useState } from 'react'
import {  Form, Button } from 'react-bootstrap'
import {useNavigate} from 'react-router-dom'
import ShortUniqueId from 'short-unique-id';
import toast from 'react-hot-toast'
import copy from 'copy-to-clipboard';
import { Carousel } from 'react-bootstrap';
import {InfoCircleFill} from 'react-bootstrap-icons'
import Footer from '../components/footer';

const Home = () => {
    const Suid = new ShortUniqueId()
    const history = useNavigate()
    const valt = 'Enter the Session Id', invalt = 'Invalid Session  Id';
    const lenvalt = 'Enter only 10 characters Session Id';
    const [uname, setuname] = useState('')    
    const [fclick, setFclick] = useState(false)
    const [sid, setSid] = useState('')
    const [sid1, setSid1] = useState('')
    const [validText, setvalidText] = useState(valt)
    const alphanumericRegex = /^[a-zA-Z0-9]+$/;
    const [interviewer, setInterviewer] = useState(false)
  

    const handleChange1 = (e) => {
        const input = e.target.value
        if (input && !alphanumericRegex.test(input)) {
            setvalidText(invalt)
            setSid1('')
            return;
        } 
        setvalidText(lenvalt)
        if(input.length>10) {
            setSid1('')
            return
        }
        setSid1(input)
    }

    const handleNameChange = (e) => {
        setuname(e.target.value)
    }

    const generateCode = (e) => {
        e.preventDefault();
        setFclick(true)
        let code  = Suid(10);
        setSid(code)
        setSid1(code)
        if (copy(code))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    const handleSwitch = (e) => {
        setInterviewer(!interviewer)
        // console.log(interviewer);
    }

    const handleJoin = async (e)=> {
        e.preventDefault()
        if(sid.length===0 && sid1.length===0) {
            toast('Please generate or fill the Session ID', {
                icon: '❕',
              });
            return;
        }
        if(uname.length==0) {
            toast.error("Name field is empty")
            return;
        }
        const roomId = sid.length>0?sid:sid1
        if(sid.length>0) {
            localStorage.setItem('init', 'true')
        }
        toast.success('Joining new Call')
        history(`/call/${roomId}`, {
            state: {
                username: uname,
                interviewer:!interviewer,
            },
        });
    }
    
    return (
        <>
        <div className='main-home'>
            <div className='cen-container'>
                {/* <Image src='/qna.png' className='img-fluid' /> */}
                <div className='lcentral l-shadow'>
                <Carousel data-bs-theme="dark" className='fwid'>
                    <Carousel.Item key='1' interval={500}>
                        <img
                        className="d-block w-100 img-fluid"
                        src="/qna.png"
                        alt="First slide"
                        />
                        <Carousel.Caption>
                        <h5 className='blk-txt'>Online Interviews</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='2' interval={750}>
                        <img alt='face motion' className="d-block w-100 img-fluid" src='/face scanner.png'/>
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Face Motion Detection</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='3' interval={750}>
                        <img alt='video' className="d-block w-100 img-fluid"  src="/interview.jpg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Video Interview</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='4' interval={750}>
                        <img alt='code editor' className="d-block w-100 img-fluid"  src="/editor.jpeg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Live Code Editor</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='5' interval={750}>
                        <img alt='live chat' className="d-block w-100 img-fluid"  src="/chat.png" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'> Live Chat</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    
                </Carousel>
                </div>
            </div>
            <div className='cen-container'>
                <div className='central r-shadow'>
                    <div className='d-flex flex-column'>
                        <div className='jcontent'>
                            <h3 className='t-color'>Welcome to <span className='myfont'>CodeCam</span></h3>

                        </div>
                        <div className='jcontent mt4'>
                            <h4>Start a New Session</h4>
                            {/* <Form> */}
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    
                                    <Form.Control 
                                        className='genInp' 
                                        type="text" 
                                        placeholder="#uniqueId" 
                                        disabled 
                                        name="sid"
                                        value={sid}
                                    />
                                        <Button className="btn-dark gen-btn"
                                            onClick={generateCode}
                                        >Generate {fclick ? 'Again' : ''}</Button>
                                </Form.Group>
                                <h4>or</h4>
                            
                        </div>
                        <div className='jcontent mt-4'>
                            <h4>Join with Code</h4>
                                <Form.Group className="mmb fin " >
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Id" 
                                        name="sid1"
                                        onChange={handleChange1}
                                        value={sid1}
                                        
                                    />
                                    <Form.Text>{validText}</Form.Text>
                                </Form.Group>
                        </div>
                        <div className='jcontent'>
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Enter your good Name" 
                       
                                        name="uname"
                                        onChange={handleNameChange}
                                        value={uname}
                                    />
                                </Form.Group>
                                <Form.Group className='ml-2'>
                                    <Form.Check
                                        type="switch"
                                        id="custom-switch"
                                        label="Join as Interviewer "
                                        value={interviewer}
                                        name='interviewer'
                                        onChange={handleSwitch}
                                        style={{display: 'inline', textAlign: 'left !important', padding: '5px'}}
                                    />
                                    <InfoCircleFill className='infoIcon'></InfoCircleFill>
                                    <span className="afterHoverText">Toggle to disable Face Monitoring</span>
                                </Form.Group>
                                <Form.Group className='mt-2'>
                                    <Button onClick={handleJoin}>Join Call</Button>
                                </Form.Group>

                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div className='disclaimer'>
            <h5>Note:</h5>
            <p>Sit in well lit room and Face camera to avoid misbehaviour notifications. <br></br>
            If your video is not visible, hard reload the page and rejoin.
            </p>
        </div>
        </>
    )
}

export default Home

--- File Index 13: src/pages/peerCall.js ---
import React, { useState, createContext, useRef, useEffect } from "react";

import toast from 'react-hot-toast';
import { useLocation, useNavigate, useParams, Navigate } from 'react-router-dom';
import { Peer } from 'peerjs'
import { Button } from "react-bootstrap";
import * as faceapi from 'face-api.js'
import Chat from "../components/pchat";
import copy from 'copy-to-clipboard';
import Editor from "../components/peditor";
import "../styles/callpage.css";
const server = process.env.REACT_APP_BACKEND_URI;
const server_host = process.env.REACT_APP_BACKEND_HOST;
const PeerCall = () => {
    const location = useLocation();
    const history = useNavigate();
    const { roomId } = useParams();

    const myName = location.state?.username
    const interviewer = location.state?.interviewer

    const MOTION_THRESHOLD = 40;
    // const myConns = new Map()
    const [myConns, setMyConns] = useState(new Map())
    const [mycalls, setMyCalls] = useState(new Set())
    const idName = new Map()


    var userMoves = 0;
    var previousLandmarks = null;
    var warned = false;
    var peer = null
    var dataStream = null
    var screenStream = null
    var myPeerId;
    var timer;

    const [allPeers, setAllPeers] = useState([])
    const [stream, setstream] = useState(null)

    const [lang, setLang] = useState('javascript')
    const [chatState, setChatState] = useState(false)
    const [displayCheck, setDisplayCheck] = useState(false)
    const [sirId, setsirId] = useState(null)
    const myVideo = useRef()
    const [code, setcode] = useState(`console.log("Live shared Code Editor")`)

    //Face motion logic
    function analyzeFaceMotions(landmarks) {
        const currentLandmarks = landmarks._positions;

        if (previousLandmarks && currentLandmarks.length == 68) {
            let totalMotion = 0, averageMotion = 0;
            for (let i = 0; i < 68; i++) {
                const dx = currentLandmarks[i]._x - previousLandmarks[i]._x;
                const dy = currentLandmarks[i]._y - previousLandmarks[i]._y;
                const distance = Math.sqrt(dx * dx + dy * dy)
                totalMotion += distance
            }
            averageMotion = totalMotion / 68;
            // console.log(averageMotion);
            if (averageMotion > MOTION_THRESHOLD) {
                toast('Face motion detected, Please concentrate on the interview', {
                    icon: '❕',
                });
                userMoves++;
            }
        }
        previousLandmarks = currentLandmarks;
    }

    async function detectFaceMotions() {
        if (!myVideo.current) return;

        timer = setInterval(async () => {
            if (warned) {
                if (sirId) { }
                // socketRef?.current?.emit(ACTIONS.BEHAVIOUR , {roomId})
                else {
                    toast.error('Please Behave until the Interviewer joins')
                }
                warned = false
                userMoves = 0
                return
            }
            if (userMoves > 12) {
                toast.error("Warning! Interviewer will be notified if movement is observed again.")
                userMoves = userMoves - 2;
                warned = true;
            }
            const detections = await faceapi.detectAllFaces(myVideo.current,
                new faceapi.TinyFaceDetectorOptions())
                .withFaceLandmarks()

            if (myVideo.current && detections.length == 0) {
                toast.error("Please sit in a well lit room and face the Webcam!")
                alert('Face cannot be detected ! Please sit in a weel lit area.')
                userMoves++;
            }
            else if (detections?.length > 1) {
                toast.error(`${detections.length} persons spotted in camera`)
                userMoves++;
            }
            else if (detections && detections.length == 1) {
                analyzeFaceMotions(detections[0].landmarks)
            } else {
                console.log(detections);
                toast.error(" Please face the webcam!")
            }

        }, 2500)
    }

    useEffect(() => {
        //take camera permission
        navigator?.mediaDevices?.getUserMedia({ video: true, audio: true })
            .then(videoStream => {
                dataStream = videoStream;
                setstream(videoStream);

                // initialize socket
                init(videoStream)

            })
            .catch((error) => {
                console.log(error);
                toast.error('Cannot connect without video and audio permissions')
                return <Navigate to="/" />;
            });
        //load faceapi Models
        loadModels()
        //socket connecting function
        const init = async (videoStream) => {
            // PeerJS functionality starts 
            var peer_params = {
                host: server_host,
                path: '/myapp',
            }
            if (server_host == 'localhost') peer_params['port'] = 3007
            peer = new Peer(peer_params)
            // once peer created join room
            peer.on('open', function (id) {
                myPeerId = id

                addVideo(videoStream, id, myName)
                if (interviewer)
                    detectFaceMotions()
                let flag = interviewer ? false : true
                fetch(`${server}join`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        username: myName,
                        peerId: id,
                        flag
                    })
                }).then(res => res.json())
                    .then(res_arr => {
                        const { data } = res_arr
                        setAllPeers(data);
                        for (const { peerId, username } of data) {

                            let conn = peer.connect(peerId)

                            conn.on("open", () => {
                                myConns.set(conn, username)
                                conn.send({ sig: 1, data: { id, name: myName } })
                                let call = peer.call(peerId, videoStream)
                                call.on('stream', (remoteStream) => {
                                    mycalls.add(call)
                                    addVideo(remoteStream, peerId, username)
                                })
                            })

                            conn.on('data', ({ sig, data }) => {
                                recvHandler(conn, sig, data)
                            })

                            conn.on('close', () => {
                                myConns.delete(conn)
                                const element = document.getElementById(peerId);
                                element?.remove();
                            })

                            if (peerId == sirId) {
                                toast.success('Interviewer Joined')
                            }
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    })

            });

            peer.on("connection", (conn) => {
                conn.on('data', ({ sig, data }) => {
                    recvHandler(conn, sig, data)
                })
                conn.on('close', () => {
                    myConns.delete(conn)
                    toast.success(`${idName[conn.peer]} left the Call`)
                    const element = document.getElementById(conn.peer);
                    element?.remove();
                })
            });

            peer.on('call', (call) => {
                call.answer(videoStream)
                mycalls.add(call)
                call.on('stream', (remoteStream) => {
                    addVideo(remoteStream, call.peer, idName[call.peer])
                })
                toast.success(`${idName[call.peer]} joined the Call`)
            })
        };

        // Clean up function to remove camera permissions and end socket
        return () => {
            const ele = document.getElementById(myPeerId)
            ele?.remove()
            clearInterval(timer)
            if (peer) {
                try {
                    fetch(`${server}leave`, {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json"
                        },
                        body: JSON.stringify({
                            roomId,
                            peerId: myPeerId,
                        })
                    })
                } catch (err) {
                    console.log(err);
                    toast.error("Coudn't leave the room at the current moment")
                }
                peer.destroy();
            }
            if (dataStream) {
                const tracks = dataStream.getTracks();
                tracks.forEach((track) => track.stop());
            }
        };
    }, []);

    //Funtions to be triggered by child components passed as props

    const sendHandler = (sig, data) => {
        for (const [conn, name] of myConns) {
            conn.send({ sig, data })
        }
    }

    const recvHandler = (conn, sig, data) => {
        switch (sig) {
            case 1: {
                //share name
                const { id, name } = data
                myConns.set(conn, name)
                idName[id] = name
                conn.send(3, { value: code })
                break;
            }
            case 2: {
                // chat msg
                const { username, msg } = data
                addRecvMsg(msg, username)
            }
            case 3: {
                // code update
                const { value } = data
                console.log(value);
                if (value != code)
                    setcode(value);
            }
            case 4: {
                // language change
                const { newLang } = data
                if (lang != newLang) setLang(newLang)
            }
            default:
                break;
        }
    }

    const addRecvMsg = (text, name = 'Unknown') => {
        setChatState(true)
        const element =
            ` <div class='receive'>
                    <div class='msg'>
                    <span class='senderName' >${name}</span>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML = element
        rdiv.setAttribute('class', 'msg-container')
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)
        if (par)
            par.scrollTop = par.scrollHeight
    }


    //Functions related to this component
    function addVideo(vstream, peerID, userName = "user") {
        const prev = document.getElementById(peerID)
        prev?.remove()
        const row = document.createElement('div')
        row.setAttribute('class', 'mt-2')
        row.setAttribute('id', peerID)

        const video = document.createElement('video')
        video.srcObject = vstream;
        video.addEventListener('loadedmetadata', () => {
            video.play()
        })
        // console.log(peerID, myPeerId);
        if (peerID == myPeerId) {
            video.muted = true
            myVideo.current = video
        }

        const span = document.createElement('span')
        span.innerText = userName
        span.setAttribute('class', 'tagName')

        row.append(video)
        row.append(span)

        const exist = document.getElementById(peerID)
        if (exist) return

        const peerDiv = document.getElementById('peerDiv')

        peerDiv?.insertBefore(row, peerDiv.children[0])

    }

    const loadModels = () => {
        Promise.all([
            faceapi.nets.faceLandmark68Net.loadFromUri("/models"),
            faceapi.nets.tinyFaceDetector.loadFromUri("/models"),
            faceapi.loadFaceLandmarkTinyModel("/models")
        ]).then(() => {
            // console.log('Models Loaded');
        }).catch(err => {
            console.log('FaceAPI modules loading error', err);
        })

    }

    const screenShareHandler = async (e) => {
        e.preventDefault()
        setDisplayCheck(!displayCheck)
        idName['screen'] = 'screen'
        navigator?.mediaDevices?.getDisplayMedia({ audio: true }).then(displStream => {
            screenStream = displStream
            addVideo(displStream, 'screen', 'Screen')
            replaceStream(displStream)
        }).catch((error) => {
            console.log(error);
        });

    }

    const replaceStream = (mediaStream) => {
        for (const call of mycalls) {
            for (let sender of call.peerConnection?.getSenders()) {
                if (sender.track.kind == "audio") {
                    if (mediaStream.getAudioTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getAudioTracks()[0]);
                    }
                }
                if (sender.track.kind == "video") {
                    if (mediaStream.getVideoTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getVideoTracks()[0]);
                    }
                }
            }
        }
    }


    function stopCapture(evt) {
        evt.preventDefault()
        setDisplayCheck(!displayCheck)
        replaceStream(stream)
        let ele = document.getElementById('screen')
        const evid = ele?.childNodes[1];
        if (evid && evid.srcObject) {
            const tracks = evid.srcObject.getTracks();
            tracks.forEach((track) => track.stop());
        }
        ele?.remove();
    }
    const copyCode = (e) => {
        e.preventDefault();
        if (copy(roomId))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    function chatHider() {
        setChatState(chatState => !chatState)
    }

    function leaveRoom() {
        if (peer) {
            try {
                fetch(`${server}leave`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        peerId: myPeerId,
                    })
                })
            } catch (err) {
                console.log(err);
                toast.error("Coudn't leave the room at the current moment")
            }
            peer.destroy()
        }
        if (dataStream) {
            const tracks = dataStream.getTracks();
            tracks.forEach((track) => track.stop());
        }
        clearInterval(timer)
        history('/');
    }

    if (!location.state) {
        return <Navigate to="/" />;
    }


    return (
        <>
            <div style={{ marginBottom: "50px", display: "flex", flexDirection: "row", justifyContent: "space-between" }} >
                <div style={{ marginTop: "20px" }}>
                    <div className='econt'>
                        <Editor
                            conns={myConns}
                            roomId={roomId}
                            onCodeChange={setcode}
                            code={code}
                            lang={lang}
                            peer={peer}
                            sendHandler={sendHandler}
                        />
                    </div>
                </div>
                <div style={{
                    display: "flex",
                    flexDirection: "column",
                    alignItems: "center",
                    width: "100%",
                    marginTop: "20px"
                }}>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        gap: "20px",
                        width: "100%",
                    }}>
                        <Button
                            onClick={leaveRoom}
                            className="mt-2 btn-danger"
                            style={{ width: '120px', margin: 'auto' }}>Leave</Button>
                        <Button
                            onClick={!displayCheck ? screenShareHandler : stopCapture} className="mt-2" style={{ width: '170px', margin: 'auto' }}>{!displayCheck ? 'Screen Share' : 'Stop Share'}</Button>
                        <Button
                            onClick={copyCode}
                            className="mt-2 btn-info"
                            style={{ width: '160px', margin: 'auto' }}>
                            Copy Session Id
                        </Button>
                        <div onClick={chatHider}>
                            {!chatState ?
                                <img style={{ width: "50px" }} src="/toggle_chat.png"></img>
                                : <></>
                            }
                        </div>
                    </div>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        marginTop: "100px",
                        width: "100%",
                        justifyContent: "space-evenly"
                    }} id="peerDiv">
                        {/* <div style={{ margin: "0px 10px" }} className='' id="users">
                        </div> */}
                    </div>
                </div>

            </div >

            {!chatState ?
                <></>
                : <>
                    <Chat
                        recvMsg={addRecvMsg}
                        conns={myConns}
                        roomId={roomId}
                        username={myName}
                        sendHandler={sendHandler}
                        peer={peer}
                        className='chatCont'
                    />
                    <img src="/close_chat.png" onClick={chatHider} className="fix_close"></img>
                </>
            }
        </>
    )
}

export default PeerCall

--- File Index 14: src/pages/addSlides.js ---
import React, {useState} from 'react'
import { Form , Button} from 'react-bootstrap'
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const AddSlides = () => {

    const [heading, setHeading] = useState("");
    const [body, setBody] = useState("");

    const handleSubmit = async (e) => {
        e.preventDefault();

        const data = {
        heading: heading,
        body: body
        };

        try {
            const response = await fetch(`${serverLink}/save_data`, {
                method: "POST",
                headers: {
                "Content-Type": "application/json"
                },
                body: JSON.stringify(data)
            });
          
            if (response.ok) {
            //   console.log("Data saved successfully");
                toast.success("Slides Updated")
            } else {
              console.error("Error saving data:", response.status);
              toast.error('Error updating')
            }
          } catch (error) {
            console.error("Error saving data:", error);
            toast.error("Error sending data")
          }
          

        // Reset form fields
        setHeading("");
        setBody("");
    };



    return (
        <div className='cen-container mt-5 lcentral l-shadow'>
            <h3>Add New Slides</h3>
            <form onSubmit={handleSubmit} >
                {/* <label htmlFor="heading">Heading:</label> */}
                {/* <input
                    type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required
                /> */}
                
                {/* <label htmlFor="body">Body:</label>
                <textarea
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                ></textarea> */}
                <Form.Group className="mb-3" >
                    <Form.Label>Heading:</Form.Label>
                    <Form.Control type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required />
                </Form.Group>
                <Form.Group className="mb-3" >
                    <Form.Label>Body:</Form.Label>
                    <Form.Control as="textarea" 
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                    rows={3} />
                </Form.Group>

                <Button type="submit">Submit</Button>
            </form>
        </div>
    )
}

export default AddSlides

--- File Index 15: src/pages/slides.js ---
import React, {useState, useEffect} from 'react'
import { Carousel } from 'react-bootstrap';
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const Slides = () => {
    let local_Data = localStorage.getItem('slides')
    const [data, setData] = useState(local_Data!=undefined ? JSON.parse(local_Data) : [])

    useEffect(() => {
        async function fetchData() {
            try {
                const res= await fetch(`${serverLink}get_data`, {
                    method: "GET",
                    headers: {
                    "Content-Type": "application/json"
                    },
                });
                const newData = await res.json();
                setData(newData)
                localStorage.setItem('slides', JSON.stringify(newData))
            } catch(error) {
                console.log(error);
                toast.error("Error receiving data")
            }
        }
        fetchData();
        
    }, [])

    return (
        <div className=''>
            <div className='cen-container l-shadow lcentral mt-5 '>
                <Carousel>
                {   
                    data.map((item, index) => (
                        
                        <Carousel.Item key={index} interval={500}>
                            <div key={index} className='f-height'>
                                <h2>{item.heading}</h2>
                                <h6>{item.body }</h6>
                            </div>
                        </Carousel.Item>

                ))}
                </Carousel>
            </div>
        </div>
    )
}

export default Slides

--- File Index 16: src/reducers/userReducer.js ---
export const initialState = null;

export const reducer = (state, action) => {
	if (action.type === "USER") {
		return action.payload;
	}if (action.type === "CLEAR") {
		return null;
	}
	return state;
};



Analyze the codebase context.
Identify the top 5-10 core most important abstractions to help those new to the codebase.

For each abstraction, provide:
1. A concise `name`.
2. A beginner-friendly `description` explaining what it is with a simple analogy, in around 100 words.
3. A list of relevant `file_indices` (integers) using the format `idx # path/comment`.

List of file indices and paths present in the context:
- 0 # README.md
- 1 # src/Actions.js
- 2 # src/index.js
- 3 # src/socket.js
- 4 # src/App.js
- 5 # src/components/navbar.js
- 6 # src/components/peditor.js
- 7 # src/components/footer.js
- 8 # src/components/pchat.js
- 9 # src/pages/NotFound.js
- 10 # src/pages/features.js
- 11 # src/pages/motivation.js
- 12 # src/pages/home.js
- 13 # src/pages/peerCall.js
- 14 # src/pages/addSlides.js
- 15 # src/pages/slides.js
- 16 # src/reducers/userReducer.js

Format the output as a YAML list of dictionaries:

```yaml
- name: |
    Query Processing
  description: |
    Explains what the abstraction does.
    It's like a central dispatcher routing requests.
  file_indices:
    - 0 # path/to/file1.py
    - 3 # path/to/related.py
- name: |
    Query Optimization
  description: |
    Another core concept, similar to a blueprint for objects.
  file_indices:
    - 5 # path/to/another.js
# ... up to 10 abstractions
```
2025-05-16 17:51:27,922 - INFO - PROMPT: 
For the project `CollabCam`:

Codebase Context:
--- File Index 0: README.md ---
# Welcome to CollabCam

**CollabCam** is a dynamic web platform specifically built to facilitate **online coding interviews**. Designed to streamline the interview process, CollabCam allows interviewers to engage with candidates in real-time through a collaborative environment that integrates coding, monitoring, and communication tools.

With CollabCam, interviewers can initiate a new session, share the unique session code with candidates, and dive into an interactive interview experience that mirrors the dynamics of an in-person coding test.

## Key Features

- ### **Collaborative Code Editor**
  CollabCam features a **live coding editor** where candidates can write, edit, and debug code directly in the interview session. The shared editor updates in real-time, allowing interviewers to observe the candidate’s approach, provide feedback, and guide them through technical questions or coding challenges seamlessly.

- ### **Video Monitoring**
  To ensure integrity and focus, CollabCam includes a **video monitoring tool** that tracks head movements of the candidate. When a candidate turns on their video, CollabCam will monitor and detect excessive head movements:
  - If notable movement is being detected, CollabCam issues a **warning**.
  - Continued movement post-warning may result in the **session being terminated**, allowing interviewers to maintain a controlled environment.

- ### **In-Session Chat**
  CollabCam also provides a built-in **chat feature** for smooth communication between the interviewer and candidate. Whether discussing the task at hand, providing hints, or addressing technical issues, the chat function ensures clear and effective interaction throughout the session.

> **Note**: Currently, CollabCam’s functionality is optimized for users connected on the same network due to limitations with the current PeerJS implementation. The development roadmap includes plans to expand this capability, enabling users to connect from any network globally.

## Future Scope

We have exciting plans for future updates that will enhance CollabCam’s utility:
- **Session Management**: Enable interviewers to conduct consecutive interviews without needing to reset the platform, making back-to-back interviews seamless.
- **Scoring and Feedback**: Add functionality for interviewers to assign scores, write feedback, and maintain a log of candidate performance over multiple sessions.

## Get Started

You can try out CollabCam and explore its features by visiting the link below:

[Access CollabCam](https://collabcam.onrender.com/)

---

With CollabCam, conducting remote technical interviews is simpler, more efficient, and fully interactive, providing a comprehensive environment for both interviewers and candidates.


--- File Index 1: src/Actions.js ---
const ACTIONS = {
    JOIN: 'join',
    JOINED: 'joined',
    DISCONNECTED: 'disconnected',
    CODE_CHANGE: 'code-change',
    SYNC_CODE: 'sync-code',
    SHARE_PEER_IDS: 'share-peer-ids',
    SIR_JOined: 'interviewer-joined',
    MONITOR: 'monitor',
    BEHAVIOUR: 'mis-behaving',
    SEND_MSG : 'send-message',
    RECV_MSG : 'receive-message',
    LANG_CHANGE: 'change-language',
    UPDATE_LAN: 'update-language',
    LEAVE: 'leave',
};

module.exports = ACTIONS;

--- File Index 2: src/index.js ---
import React from 'react';
import {createRoot} from 'react-dom/client'
import './index.css';
import App from './App';
// import { ContextProvider } from './Context';
import 'bootstrap/dist/css/bootstrap.min.css';

const root_element = document.getElementById('root');
const root= createRoot(root_element)
root.render(<App />);

// root.render(
//     <React.StrictMode>
//         {/* <ContextProvider> */}
//             <App/>
//         {/* </ContextProvider> */}
//     </React.StrictMode>
// );


--- File Index 3: src/socket.js ---
import { io } from 'socket.io-client';

export const initSocket = async () => {
    const options = {
        'force new connection': true,
        reconnectionAttempt: 'Infinity',
        timeout: 10000,
        transports: ['websocket'],
    };
    // console.log('conecting to socket');
    // return io('https://c2f-api-vineethkumarm.koyeb.app:8000', options);
    return io('https://CodeCamapi.onrender.com', options);
    // return io('https://amazing-croissant-5bbe89.netlify.app/', options);
    // return io('https://c2f-api.el.r.appspot.com', options);
    // return io('http://localhost:3007',options);
    // return io()
    console.log(process.env);
    return io(process.env.REACT_APP_BACKEND_URI, options);
};

--- File Index 4: src/App.js ---
import './App.css';
import React,{createContext, useReducer} from 'react';
import {Route , BrowserRouter as Router,Routes} from 'react-router-dom'
import { initialState, reducer } from './reducers/userReducer';
import 'react-bootstrap'
//components and pages
import MyNavbar from './components/navbar';
import Home from './pages/home';
import NotFound from './pages/NotFound';
// import Testing from './pages/testing';
import { Toaster } from 'react-hot-toast';
import Slides from './pages/slides';
import AddSlides from './pages/addSlides';
import PeerCall from './pages/peerCall';
import Motivation from './pages/motivation';
import Features from './pages/features';
import Footer from './components/footer';

export const UserContext = createContext();

function App() {


	const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <UserContext.Provider value={{state,dispatch}}>
            <div>
                <Toaster 
                    position='top-right'
                />
            </div>
            <Router >
                <div className='wrapper' >

                <div className='App'>
                    <MyNavbar/>
                    <div className='content d-flex'>
                        <Routes>
                                <Route eaxct path="/" element={<Home/>} />
                                {/* <Route exact path="/call/:roomId" element={<CallPage/>} /> */}
                                <Route exact path="/call/:roomId" element={<PeerCall/>} />
                                <Route exact path='/slides' element={<Slides/>} />
                                <Route exact path='/add' element={<AddSlides/>} />
                                {/* <Route path="/chat" element={<Chat/>} /> */}
                                <Route exact path='/motivation' element={<Motivation />} />
                                {/* <Route exact path='/features' element={<Features />} /> */}
                                <Route path="*" element={<NotFound/>} />
                        </Routes>
                    </div>
                </div>
                    {/* <Footer /> */}
                </div>
            </Router>
         </UserContext.Provider>
		
	
	);
}

export default App;


--- File Index 5: src/components/navbar.js ---
import React,{ useContext } from "react";
import { Link } from "react-router-dom";
import { UserContext } from "../App";
import { useNavigate } from "react-router-dom";
import { NavbarBrand } from "react-bootstrap";
import "../styles/navbar.css"

const Navbar = () => {
	let { state, dispatch } = useContext(UserContext);
	const history = useNavigate()
	let jwt = localStorage.getItem("jwt")
	if(jwt) {
		state=true;
	}


	const renderList = () => {
		if (state) {
			return [
				<>
					<Link key='profile' className="navlink" to="/profile">Profile</Link>
					<Link key='dsf' className="navlink" to="/newPost">new post</Link>
					<button key={jwt.slice(0,3)} className="btn btn-primary" onClick={() => {
							localStorage.clear();
							dispatch({type:"CLEAR"})
							history('/')
						}
						}>
						Logout
					</button>
				
				</>
			];
		} else {
			return [
				<>
                    {/* <Link to='/testing' className="navlink">Test</Link>  */}
					{/* <Link key='login' className="navlink" to="/login">Login</Link> */}
					{/* <Link key='register' className="navlink" to="/register">Register</Link> */}
					<Link key='slides' className="navlink" to="/motivation">Motivation</Link>
					{/* <Link key='slides' className="navlink" to="/features">Features</Link> */}
					<Link key='slides' className="navlink" to="/slides">Revision</Link>
					
				</>,
			];
		}
	};

	return (
		<nav className="main-nav">
			{/* <div className="logo" ><h3 href="/">CodeCam</h3></div> */}
            <NavbarBrand className="logo">
                <Link key='logo' to="/">
                    CodeCam
                    {/* Test */}
                </Link>
            </NavbarBrand>
			<div className="navlinks">{renderList()}</div>
		</nav>
	);
};

export default Navbar;

--- File Index 6: src/components/peditor.js ---
import React, { useEffect, useState, useRef, useMemo } from 'react'
import { useLocation } from 'react-router-dom';
import { Button, Form } from 'react-bootstrap';
// codemirror components
import { useCodeMirror } from '@uiw/react-codemirror';
import Select from 'react-select';

// import languages 
import { javascript } from '@codemirror/lang-javascript';
import { cpp } from '@codemirror/lang-cpp';
import { python } from '@codemirror/lang-python';
import { java } from '@codemirror/lang-java';

// import themes 
import { githubDark } from '@uiw/codemirror-theme-github';
import { xcodeDark, xcodeLight } from '@uiw/codemirror-theme-xcode';
import { eclipse } from '@uiw/codemirror-theme-eclipse';
import { abcdef } from '@uiw/codemirror-theme-abcdef';
import { solarizedDark } from '@uiw/codemirror-theme-solarized';
import "../styles/editor.css"
import { toast } from 'react-hot-toast';

var qs = require('qs');


// const extensions = [javascript()]


const Editor = ({ sendHandler, roomId, onCodeChange, code, lang }) => {
    const history = useLocation()
    const location = useLocation()
    const uname = location?.state?.username
    const [theme, setTheme] = useState(githubDark);
    // const [code, se==1ode] = useState(code);
    const [selectValue, setSelectValue] = useState('javascript')
    const [extensions, setExtensions] = useState([javascript()])
    const [placeholder, setPlaceholder] = useState('Please enter the code.');
    const [input, setInput] = useState('')
    const [output, setOutput] = useState('')
    const [ran, setran] = useState(false)
    const [tc, setTc] = useState(true)
    const thememap = new Map()
    const langnMap = new Map()

    const editorRef = useRef(null)
    const { setContainer } = useCodeMirror({
        container: editorRef.current,
        extensions,
        value: code,
        theme,
        editable: true,
        height: `70vh`,
        width: `45vw`,
        basicSetup: {
            foldGutter: false,
            dropCursor: false,
            indentOnInput: false,
        },
        options: {
            autoComplete: true,
        },
        placeholder: placeholder,
        style: {
            position: `relative`,
            zIndex: `999`,
            borderRadius: `10px`,
        },
        onChange: (value) => {
            onCodeChange(value)
            sendHandler(3, { value })
        }

    })


    const themeInit = () => {
        thememap.set("githubDark", githubDark)
        thememap.set("xcodeDark", xcodeDark)
        thememap.set("eclipse", eclipse)
        thememap.set("xcodeLight", xcodeLight)
        thememap.set("abcdef", abcdef)
        thememap.set("solarizedDark", solarizedDark)
    }

    const langInit = () => {
        langnMap.set('java', java)
        langnMap.set('cpp', cpp)
        langnMap.set("javascript", javascript)
        langnMap.set('python', python)
    }


    const handleThemeChange = (event) => {
        setTheme(thememap.get(event.value));
    };

    const handleLanguageChange = (event) => {
        setExtensions([langnMap.get(event.value)()]);

        sendHandler(4, { newLang: event.value })
        setSelectValue(event.value)
    };

    themeInit()
    langInit()

    function langCode(e) {
        if (e == 'javascript') return 'js';
        else if (e == 'python') return 'py';
        return e;
    }

    useEffect(() => {
        if (!editorRef.current) {
            alert('error loading editor')
            history('/')
        }
        if (editorRef.current) {
            setContainer(editorRef.current)
        }

    }, [editorRef.current])

    useEffect(() => {
        if (lang != selectValue && lang) {
            setSelectValue(lang)
            setExtensions([langnMap.get(lang)()]);
        }
    }, [lang])

    const compile = (e) => {
        e.preventDefault();
        var data = qs.stringify({
            'code': code,
            'language': langCode(selectValue),
            'input': input
        });
        var config = {
            method: 'post',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            },
            body: data
        };
        fetch('https://api.codex.jaagrav.in', config)
            .then(res => res.json())
            .then(data => {
                if (data['error'].length == 0) {
                    setTc(true)
                    toast.success("compiled sucessfully")
                    setOutput(data['output'])
                }
                else {
                    setTc(false)
                    toast.error("compilation error")
                    setOutput(data['error'])
                }
            })
            .catch(function (error) {
                console.log(error);
            });
    }

    const handleInputChange = (e) => {
        setInput(e.target.value)
    }
    const handleOutputChange = (e) => {
        setOutput(e.target.value)
    }

    const themeOptions = [
        { value: "githubDark", label: "Github Dark" },
        { value: "eclipse", label: "Eclipse" },
        { value: "xcodelight", label: "X Code Light" },
        { value: "xcodedark", label: "X Code Dark" },
        { value: "solarizeddark", label: "Solarized Dark" },
        { value: "abcdef", label: "ABCDEFs" },
    ]

    const langOptions = [
        { value: "javascript", label: "JavaScript" },
        { value: "java", label: "Java" },
        { value: "cpp", label: "C++" },
        { value: "python", label: "Python" }
    ]

    return (
        <div className='editorcomponent'>
            <div style={{
                display: "flex",
                gap: "10px",
                flexDirection: "column"
            }}>
                <div>
                    <span>Theme:</span>
                    <Select
                        onChange={handleThemeChange}
                        classNamePrefix="select"
                        defaultValue={themeOptions[0]}
                        name="color"
                        options={themeOptions}
                    />
                </div>
                <div>
                    <span>Language:</span>
                    <Select
                        onChange={handleLanguageChange}
                        className="basic-single"
                        classNamePrefix="select"
                        defaultValue={langOptions[0]}
                        name="color"
                        options={langOptions}
                    />
                </div>
                <button className='run ' onClick={compile} >Run</button>
            </div>
            <div style={{marginTop: "20px"}} ref={editorRef} className='ide' ></div>
            <div style={{display: "flex", flexDirection: "row", justifyContent: "space-between", marginTop: "20px"}} className='iodiv d-flex flex-wrap'>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Input</Form.Label>
                    <Form.Control as="textarea" rows={2} value={input} onChange={handleInputChange} />
                </Form.Group>
                <Form.Group style={{width: "48%"}} controlId="exampleForm.ControlTextarea1">
                    <Form.Label>Output</Form.Label>
                    <Form.Control as="textarea" rows={2} value={output} className={tc ? 'ctxt' : 'etxt'} onChange={handleOutputChange} disabled />
                </Form.Group>

            </div>
        </div>
    );
}

export default Editor

--- File Index 7: src/components/footer.js ---
import React from 'react'
import "../styles/footer.css"
const Footer = () => {
  return (
    <footer className="page-footer font-small bg-dark text-center text-white pt-2">

        <div className="footer-copyright text-center py-3">
            <h5>Made With ❤️ by Aaditya Kumar </h5>
        </div>

    </footer>
    )
}

export default Footer


--- File Index 8: src/components/pchat.js ---
import React, {useEffect, useState} from 'react'
import { Form } from 'react-bootstrap'

import "../styles/chat.css"

const Chat = ({sendHandler, roomId, username}) => {

    const [msg, setmsg] = useState('')

    const handleChange = (e) => {
        setmsg(e.target.value)
    }
    const handleSend = (e) => {
        e.preventDefault();
        if(msg.length==0) return;
        sendHandler(2,{username, msg})
        addSendMsg(msg)
        setmsg('')
    }
    
    const addSendMsg = (text) => {
        const element = 
            ` <div class='send'>
                    <div class='msg'>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML=element
        rdiv.setAttribute('class', 'msg-container')
        
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)  
        par.scrollTop = par.scrollHeight  
    }


    return (
        <div className='chatCont mt5'>
            <div className='chatBox'>
                <div className='scroller' id='msg-div'>
                </div>
                <div className='minp'>
                    <Form onSubmit={handleSend}>

                    
                        <Form.Control
                            type="text" 
                            placeholder="Enter message" 
                            name="msg"
                            onChange={handleChange}
                            value={msg}
                            className='finp inlin'
                        />
                        {/* <Button className='inlin' onClick={handleSend}> Send</Button> */}
                    </Form>
                </div>
            </div>
        </div>
    )
}

export default Chat

--- File Index 9: src/pages/NotFound.js ---
import React from 'react'

const NotFound = () => {
  return (
    <div className='errorpage'><span>Sorry, the request resources are not available on this website</span></div>
  )
}

export default NotFound

--- File Index 10: src/pages/features.js ---
import React from 'react'

const Features = () => {
  return (
    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Features</h2>
        <ul>
            <li>Video Chatting</li>
            <li>Screen Sharing</li>
            <li>Face Monitoring</li>
            <li>Collaborative Code Editor</li>
            <li>Online Compiler</li>
        </ul>
    </div>
  )
}

export default Features

--- File Index 11: src/pages/motivation.js ---
import React from 'react'

const Motivation = () => {
  return (

    <div className='cen-container  mt-5 l-shadow mot'>
        <h2>Motivation</h2>
        <p>Introducing the ultimate solution for seamless and efficient online interviews - this innovative 
            project brings together the power of video chat, code editor, and compiler all in one platform!
             No more juggling between multiple tabs or applications during interviews. With this website, you 
             can now focus solely on showcasing your skills and acing those coding challenges. Embrace the 
             ease of navigating through the interview process effortlessly, while impressing your potential 
             employers with your coding prowess.

        </p>
    </div>
  )
}

export default Motivation

--- File Index 12: src/pages/home.js ---
import React, { useState } from 'react'
import {  Form, Button } from 'react-bootstrap'
import {useNavigate} from 'react-router-dom'
import ShortUniqueId from 'short-unique-id';
import toast from 'react-hot-toast'
import copy from 'copy-to-clipboard';
import { Carousel } from 'react-bootstrap';
import {InfoCircleFill} from 'react-bootstrap-icons'
import Footer from '../components/footer';

const Home = () => {
    const Suid = new ShortUniqueId()
    const history = useNavigate()
    const valt = 'Enter the Session Id', invalt = 'Invalid Session  Id';
    const lenvalt = 'Enter only 10 characters Session Id';
    const [uname, setuname] = useState('')    
    const [fclick, setFclick] = useState(false)
    const [sid, setSid] = useState('')
    const [sid1, setSid1] = useState('')
    const [validText, setvalidText] = useState(valt)
    const alphanumericRegex = /^[a-zA-Z0-9]+$/;
    const [interviewer, setInterviewer] = useState(false)
  

    const handleChange1 = (e) => {
        const input = e.target.value
        if (input && !alphanumericRegex.test(input)) {
            setvalidText(invalt)
            setSid1('')
            return;
        } 
        setvalidText(lenvalt)
        if(input.length>10) {
            setSid1('')
            return
        }
        setSid1(input)
    }

    const handleNameChange = (e) => {
        setuname(e.target.value)
    }

    const generateCode = (e) => {
        e.preventDefault();
        setFclick(true)
        let code  = Suid(10);
        setSid(code)
        setSid1(code)
        if (copy(code))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    const handleSwitch = (e) => {
        setInterviewer(!interviewer)
        // console.log(interviewer);
    }

    const handleJoin = async (e)=> {
        e.preventDefault()
        if(sid.length===0 && sid1.length===0) {
            toast('Please generate or fill the Session ID', {
                icon: '❕',
              });
            return;
        }
        if(uname.length==0) {
            toast.error("Name field is empty")
            return;
        }
        const roomId = sid.length>0?sid:sid1
        if(sid.length>0) {
            localStorage.setItem('init', 'true')
        }
        toast.success('Joining new Call')
        history(`/call/${roomId}`, {
            state: {
                username: uname,
                interviewer:!interviewer,
            },
        });
    }
    
    return (
        <>
        <div className='main-home'>
            <div className='cen-container'>
                {/* <Image src='/qna.png' className='img-fluid' /> */}
                <div className='lcentral l-shadow'>
                <Carousel data-bs-theme="dark" className='fwid'>
                    <Carousel.Item key='1' interval={500}>
                        <img
                        className="d-block w-100 img-fluid"
                        src="/qna.png"
                        alt="First slide"
                        />
                        <Carousel.Caption>
                        <h5 className='blk-txt'>Online Interviews</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='2' interval={750}>
                        <img alt='face motion' className="d-block w-100 img-fluid" src='/face scanner.png'/>
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Face Motion Detection</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='3' interval={750}>
                        <img alt='video' className="d-block w-100 img-fluid"  src="/interview.jpg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Video Interview</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='4' interval={750}>
                        <img alt='code editor' className="d-block w-100 img-fluid"  src="/editor.jpeg" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'>Live Code Editor</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    <Carousel.Item key='5' interval={750}>
                        <img alt='live chat' className="d-block w-100 img-fluid"  src="/chat.png" />
                        <Carousel.Caption>
                            <h5 className='blk-txt'> Live Chat</h5>
                        </Carousel.Caption>
                    </Carousel.Item>
                    
                </Carousel>
                </div>
            </div>
            <div className='cen-container'>
                <div className='central r-shadow'>
                    <div className='d-flex flex-column'>
                        <div className='jcontent'>
                            <h3 className='t-color'>Welcome to <span className='myfont'>CodeCam</span></h3>

                        </div>
                        <div className='jcontent mt4'>
                            <h4>Start a New Session</h4>
                            {/* <Form> */}
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    
                                    <Form.Control 
                                        className='genInp' 
                                        type="text" 
                                        placeholder="#uniqueId" 
                                        disabled 
                                        name="sid"
                                        value={sid}
                                    />
                                        <Button className="btn-dark gen-btn"
                                            onClick={generateCode}
                                        >Generate {fclick ? 'Again' : ''}</Button>
                                </Form.Group>
                                <h4>or</h4>
                            
                        </div>
                        <div className='jcontent mt-4'>
                            <h4>Join with Code</h4>
                                <Form.Group className="mmb fin " >
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Id" 
                                        name="sid1"
                                        onChange={handleChange1}
                                        value={sid1}
                                        
                                    />
                                    <Form.Text>{validText}</Form.Text>
                                </Form.Group>
                        </div>
                        <div className='jcontent'>
                                <Form.Group className="mmb fin d-flex" controlId="">
                                    <Form.Control 
                                        type="text" 
                                        placeholder="Enter your good Name" 
                       
                                        name="uname"
                                        onChange={handleNameChange}
                                        value={uname}
                                    />
                                </Form.Group>
                                <Form.Group className='ml-2'>
                                    <Form.Check
                                        type="switch"
                                        id="custom-switch"
                                        label="Join as Interviewer "
                                        value={interviewer}
                                        name='interviewer'
                                        onChange={handleSwitch}
                                        style={{display: 'inline', textAlign: 'left !important', padding: '5px'}}
                                    />
                                    <InfoCircleFill className='infoIcon'></InfoCircleFill>
                                    <span className="afterHoverText">Toggle to disable Face Monitoring</span>
                                </Form.Group>
                                <Form.Group className='mt-2'>
                                    <Button onClick={handleJoin}>Join Call</Button>
                                </Form.Group>

                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div className='disclaimer'>
            <h5>Note:</h5>
            <p>Sit in well lit room and Face camera to avoid misbehaviour notifications. <br></br>
            If your video is not visible, hard reload the page and rejoin.
            </p>
        </div>
        </>
    )
}

export default Home

--- File Index 13: src/pages/peerCall.js ---
import React, { useState, createContext, useRef, useEffect } from "react";

import toast from 'react-hot-toast';
import { useLocation, useNavigate, useParams, Navigate } from 'react-router-dom';
import { Peer } from 'peerjs'
import { Button } from "react-bootstrap";
import * as faceapi from 'face-api.js'
import Chat from "../components/pchat";
import copy from 'copy-to-clipboard';
import Editor from "../components/peditor";
import "../styles/callpage.css";
const server = process.env.REACT_APP_BACKEND_URI;
const server_host = process.env.REACT_APP_BACKEND_HOST;
const PeerCall = () => {
    const location = useLocation();
    const history = useNavigate();
    const { roomId } = useParams();

    const myName = location.state?.username
    const interviewer = location.state?.interviewer

    const MOTION_THRESHOLD = 40;
    // const myConns = new Map()
    const [myConns, setMyConns] = useState(new Map())
    const [mycalls, setMyCalls] = useState(new Set())
    const idName = new Map()


    var userMoves = 0;
    var previousLandmarks = null;
    var warned = false;
    var peer = null
    var dataStream = null
    var screenStream = null
    var myPeerId;
    var timer;

    const [allPeers, setAllPeers] = useState([])
    const [stream, setstream] = useState(null)

    const [lang, setLang] = useState('javascript')
    const [chatState, setChatState] = useState(false)
    const [displayCheck, setDisplayCheck] = useState(false)
    const [sirId, setsirId] = useState(null)
    const myVideo = useRef()
    const [code, setcode] = useState(`console.log("Live shared Code Editor")`)

    //Face motion logic
    function analyzeFaceMotions(landmarks) {
        const currentLandmarks = landmarks._positions;

        if (previousLandmarks && currentLandmarks.length == 68) {
            let totalMotion = 0, averageMotion = 0;
            for (let i = 0; i < 68; i++) {
                const dx = currentLandmarks[i]._x - previousLandmarks[i]._x;
                const dy = currentLandmarks[i]._y - previousLandmarks[i]._y;
                const distance = Math.sqrt(dx * dx + dy * dy)
                totalMotion += distance
            }
            averageMotion = totalMotion / 68;
            // console.log(averageMotion);
            if (averageMotion > MOTION_THRESHOLD) {
                toast('Face motion detected, Please concentrate on the interview', {
                    icon: '❕',
                });
                userMoves++;
            }
        }
        previousLandmarks = currentLandmarks;
    }

    async function detectFaceMotions() {
        if (!myVideo.current) return;

        timer = setInterval(async () => {
            if (warned) {
                if (sirId) { }
                // socketRef?.current?.emit(ACTIONS.BEHAVIOUR , {roomId})
                else {
                    toast.error('Please Behave until the Interviewer joins')
                }
                warned = false
                userMoves = 0
                return
            }
            if (userMoves > 12) {
                toast.error("Warning! Interviewer will be notified if movement is observed again.")
                userMoves = userMoves - 2;
                warned = true;
            }
            const detections = await faceapi.detectAllFaces(myVideo.current,
                new faceapi.TinyFaceDetectorOptions())
                .withFaceLandmarks()

            if (myVideo.current && detections.length == 0) {
                toast.error("Please sit in a well lit room and face the Webcam!")
                alert('Face cannot be detected ! Please sit in a weel lit area.')
                userMoves++;
            }
            else if (detections?.length > 1) {
                toast.error(`${detections.length} persons spotted in camera`)
                userMoves++;
            }
            else if (detections && detections.length == 1) {
                analyzeFaceMotions(detections[0].landmarks)
            } else {
                console.log(detections);
                toast.error(" Please face the webcam!")
            }

        }, 2500)
    }

    useEffect(() => {
        //take camera permission
        navigator?.mediaDevices?.getUserMedia({ video: true, audio: true })
            .then(videoStream => {
                dataStream = videoStream;
                setstream(videoStream);

                // initialize socket
                init(videoStream)

            })
            .catch((error) => {
                console.log(error);
                toast.error('Cannot connect without video and audio permissions')
                return <Navigate to="/" />;
            });
        //load faceapi Models
        loadModels()
        //socket connecting function
        const init = async (videoStream) => {
            // PeerJS functionality starts 
            var peer_params = {
                host: server_host,
                path: '/myapp',
            }
            if (server_host == 'localhost') peer_params['port'] = 3007
            peer = new Peer(peer_params)
            // once peer created join room
            peer.on('open', function (id) {
                myPeerId = id

                addVideo(videoStream, id, myName)
                if (interviewer)
                    detectFaceMotions()
                let flag = interviewer ? false : true
                fetch(`${server}join`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        username: myName,
                        peerId: id,
                        flag
                    })
                }).then(res => res.json())
                    .then(res_arr => {
                        const { data } = res_arr
                        setAllPeers(data);
                        for (const { peerId, username } of data) {

                            let conn = peer.connect(peerId)

                            conn.on("open", () => {
                                myConns.set(conn, username)
                                conn.send({ sig: 1, data: { id, name: myName } })
                                let call = peer.call(peerId, videoStream)
                                call.on('stream', (remoteStream) => {
                                    mycalls.add(call)
                                    addVideo(remoteStream, peerId, username)
                                })
                            })

                            conn.on('data', ({ sig, data }) => {
                                recvHandler(conn, sig, data)
                            })

                            conn.on('close', () => {
                                myConns.delete(conn)
                                const element = document.getElementById(peerId);
                                element?.remove();
                            })

                            if (peerId == sirId) {
                                toast.success('Interviewer Joined')
                            }
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    })

            });

            peer.on("connection", (conn) => {
                conn.on('data', ({ sig, data }) => {
                    recvHandler(conn, sig, data)
                })
                conn.on('close', () => {
                    myConns.delete(conn)
                    toast.success(`${idName[conn.peer]} left the Call`)
                    const element = document.getElementById(conn.peer);
                    element?.remove();
                })
            });

            peer.on('call', (call) => {
                call.answer(videoStream)
                mycalls.add(call)
                call.on('stream', (remoteStream) => {
                    addVideo(remoteStream, call.peer, idName[call.peer])
                })
                toast.success(`${idName[call.peer]} joined the Call`)
            })
        };

        // Clean up function to remove camera permissions and end socket
        return () => {
            const ele = document.getElementById(myPeerId)
            ele?.remove()
            clearInterval(timer)
            if (peer) {
                try {
                    fetch(`${server}leave`, {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json"
                        },
                        body: JSON.stringify({
                            roomId,
                            peerId: myPeerId,
                        })
                    })
                } catch (err) {
                    console.log(err);
                    toast.error("Coudn't leave the room at the current moment")
                }
                peer.destroy();
            }
            if (dataStream) {
                const tracks = dataStream.getTracks();
                tracks.forEach((track) => track.stop());
            }
        };
    }, []);

    //Funtions to be triggered by child components passed as props

    const sendHandler = (sig, data) => {
        for (const [conn, name] of myConns) {
            conn.send({ sig, data })
        }
    }

    const recvHandler = (conn, sig, data) => {
        switch (sig) {
            case 1: {
                //share name
                const { id, name } = data
                myConns.set(conn, name)
                idName[id] = name
                conn.send(3, { value: code })
                break;
            }
            case 2: {
                // chat msg
                const { username, msg } = data
                addRecvMsg(msg, username)
            }
            case 3: {
                // code update
                const { value } = data
                console.log(value);
                if (value != code)
                    setcode(value);
            }
            case 4: {
                // language change
                const { newLang } = data
                if (lang != newLang) setLang(newLang)
            }
            default:
                break;
        }
    }

    const addRecvMsg = (text, name = 'Unknown') => {
        setChatState(true)
        const element =
            ` <div class='receive'>
                    <div class='msg'>
                    <span class='senderName' >${name}</span>
                       ${text}
                    </div>
                </div>`;
        const rdiv = document.createElement('div')
        rdiv.innerHTML = element
        rdiv.setAttribute('class', 'msg-container')
        const par = document.getElementById('msg-div')
        par?.appendChild(rdiv)
        if (par)
            par.scrollTop = par.scrollHeight
    }


    //Functions related to this component
    function addVideo(vstream, peerID, userName = "user") {
        const prev = document.getElementById(peerID)
        prev?.remove()
        const row = document.createElement('div')
        row.setAttribute('class', 'mt-2')
        row.setAttribute('id', peerID)

        const video = document.createElement('video')
        video.srcObject = vstream;
        video.addEventListener('loadedmetadata', () => {
            video.play()
        })
        // console.log(peerID, myPeerId);
        if (peerID == myPeerId) {
            video.muted = true
            myVideo.current = video
        }

        const span = document.createElement('span')
        span.innerText = userName
        span.setAttribute('class', 'tagName')

        row.append(video)
        row.append(span)

        const exist = document.getElementById(peerID)
        if (exist) return

        const peerDiv = document.getElementById('peerDiv')

        peerDiv?.insertBefore(row, peerDiv.children[0])

    }

    const loadModels = () => {
        Promise.all([
            faceapi.nets.faceLandmark68Net.loadFromUri("/models"),
            faceapi.nets.tinyFaceDetector.loadFromUri("/models"),
            faceapi.loadFaceLandmarkTinyModel("/models")
        ]).then(() => {
            // console.log('Models Loaded');
        }).catch(err => {
            console.log('FaceAPI modules loading error', err);
        })

    }

    const screenShareHandler = async (e) => {
        e.preventDefault()
        setDisplayCheck(!displayCheck)
        idName['screen'] = 'screen'
        navigator?.mediaDevices?.getDisplayMedia({ audio: true }).then(displStream => {
            screenStream = displStream
            addVideo(displStream, 'screen', 'Screen')
            replaceStream(displStream)
        }).catch((error) => {
            console.log(error);
        });

    }

    const replaceStream = (mediaStream) => {
        for (const call of mycalls) {
            for (let sender of call.peerConnection?.getSenders()) {
                if (sender.track.kind == "audio") {
                    if (mediaStream.getAudioTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getAudioTracks()[0]);
                    }
                }
                if (sender.track.kind == "video") {
                    if (mediaStream.getVideoTracks().length > 0) {
                        sender.replaceTrack(mediaStream.getVideoTracks()[0]);
                    }
                }
            }
        }
    }


    function stopCapture(evt) {
        evt.preventDefault()
        setDisplayCheck(!displayCheck)
        replaceStream(stream)
        let ele = document.getElementById('screen')
        const evid = ele?.childNodes[1];
        if (evid && evid.srcObject) {
            const tracks = evid.srcObject.getTracks();
            tracks.forEach((track) => track.stop());
        }
        ele?.remove();
    }
    const copyCode = (e) => {
        e.preventDefault();
        if (copy(roomId))
            toast.success('Session ID copied')
        else toast.error('Cannot copy to clipboard')
    }

    function chatHider() {
        setChatState(chatState => !chatState)
    }

    function leaveRoom() {
        if (peer) {
            try {
                fetch(`${server}leave`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        roomId,
                        peerId: myPeerId,
                    })
                })
            } catch (err) {
                console.log(err);
                toast.error("Coudn't leave the room at the current moment")
            }
            peer.destroy()
        }
        if (dataStream) {
            const tracks = dataStream.getTracks();
            tracks.forEach((track) => track.stop());
        }
        clearInterval(timer)
        history('/');
    }

    if (!location.state) {
        return <Navigate to="/" />;
    }


    return (
        <>
            <div style={{ marginBottom: "50px", display: "flex", flexDirection: "row", justifyContent: "space-between" }} >
                <div style={{ marginTop: "20px" }}>
                    <div className='econt'>
                        <Editor
                            conns={myConns}
                            roomId={roomId}
                            onCodeChange={setcode}
                            code={code}
                            lang={lang}
                            peer={peer}
                            sendHandler={sendHandler}
                        />
                    </div>
                </div>
                <div style={{
                    display: "flex",
                    flexDirection: "column",
                    alignItems: "center",
                    width: "100%",
                    marginTop: "20px"
                }}>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        gap: "20px",
                        width: "100%",
                    }}>
                        <Button
                            onClick={leaveRoom}
                            className="mt-2 btn-danger"
                            style={{ width: '120px', margin: 'auto' }}>Leave</Button>
                        <Button
                            onClick={!displayCheck ? screenShareHandler : stopCapture} className="mt-2" style={{ width: '170px', margin: 'auto' }}>{!displayCheck ? 'Screen Share' : 'Stop Share'}</Button>
                        <Button
                            onClick={copyCode}
                            className="mt-2 btn-info"
                            style={{ width: '160px', margin: 'auto' }}>
                            Copy Session Id
                        </Button>
                        <div onClick={chatHider}>
                            {!chatState ?
                                <img style={{ width: "50px" }} src="/toggle_chat.png"></img>
                                : <></>
                            }
                        </div>
                    </div>
                    <div style={{
                        display: "flex",
                        flexDirection: "row",
                        marginTop: "100px",
                        width: "100%",
                        justifyContent: "space-evenly"
                    }} id="peerDiv">
                        {/* <div style={{ margin: "0px 10px" }} className='' id="users">
                        </div> */}
                    </div>
                </div>

            </div >

            {!chatState ?
                <></>
                : <>
                    <Chat
                        recvMsg={addRecvMsg}
                        conns={myConns}
                        roomId={roomId}
                        username={myName}
                        sendHandler={sendHandler}
                        peer={peer}
                        className='chatCont'
                    />
                    <img src="/close_chat.png" onClick={chatHider} className="fix_close"></img>
                </>
            }
        </>
    )
}

export default PeerCall

--- File Index 14: src/pages/addSlides.js ---
import React, {useState} from 'react'
import { Form , Button} from 'react-bootstrap'
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const AddSlides = () => {

    const [heading, setHeading] = useState("");
    const [body, setBody] = useState("");

    const handleSubmit = async (e) => {
        e.preventDefault();

        const data = {
        heading: heading,
        body: body
        };

        try {
            const response = await fetch(`${serverLink}/save_data`, {
                method: "POST",
                headers: {
                "Content-Type": "application/json"
                },
                body: JSON.stringify(data)
            });
          
            if (response.ok) {
            //   console.log("Data saved successfully");
                toast.success("Slides Updated")
            } else {
              console.error("Error saving data:", response.status);
              toast.error('Error updating')
            }
          } catch (error) {
            console.error("Error saving data:", error);
            toast.error("Error sending data")
          }
          

        // Reset form fields
        setHeading("");
        setBody("");
    };



    return (
        <div className='cen-container mt-5 lcentral l-shadow'>
            <h3>Add New Slides</h3>
            <form onSubmit={handleSubmit} >
                {/* <label htmlFor="heading">Heading:</label> */}
                {/* <input
                    type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required
                /> */}
                
                {/* <label htmlFor="body">Body:</label>
                <textarea
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                ></textarea> */}
                <Form.Group className="mb-3" >
                    <Form.Label>Heading:</Form.Label>
                    <Form.Control type="text"
                    id="heading"
                    value={heading}
                    onChange={(e) => setHeading(e.target.value)}
                    required />
                </Form.Group>
                <Form.Group className="mb-3" >
                    <Form.Label>Body:</Form.Label>
                    <Form.Control as="textarea" 
                    id="body"
                    value={body}
                    onChange={(e) => setBody(e.target.value)}
                    required
                    rows={3} />
                </Form.Group>

                <Button type="submit">Submit</Button>
            </form>
        </div>
    )
}

export default AddSlides

--- File Index 15: src/pages/slides.js ---
import React, {useState, useEffect} from 'react'
import { Carousel } from 'react-bootstrap';
import toast from 'react-hot-toast'
const serverLink = process.env.REACT_APP_BACKEND_URI
const Slides = () => {
    let local_Data = localStorage.getItem('slides')
    const [data, setData] = useState(local_Data!=undefined ? JSON.parse(local_Data) : [])

    useEffect(() => {
        async function fetchData() {
            try {
                const res= await fetch(`${serverLink}get_data`, {
                    method: "GET",
                    headers: {
                    "Content-Type": "application/json"
                    },
                });
                const newData = await res.json();
                setData(newData)
                localStorage.setItem('slides', JSON.stringify(newData))
            } catch(error) {
                console.log(error);
                toast.error("Error receiving data")
            }
        }
        fetchData();
        
    }, [])

    return (
        <div className=''>
            <div className='cen-container l-shadow lcentral mt-5 '>
                <Carousel>
                {   
                    data.map((item, index) => (
                        
                        <Carousel.Item key={index} interval={500}>
                            <div key={index} className='f-height'>
                                <h2>{item.heading}</h2>
                                <h6>{item.body }</h6>
                            </div>
                        </Carousel.Item>

                ))}
                </Carousel>
            </div>
        </div>
    )
}

export default Slides

--- File Index 16: src/reducers/userReducer.js ---
export const initialState = null;

export const reducer = (state, action) => {
	if (action.type === "USER") {
		return action.payload;
	}if (action.type === "CLEAR") {
		return null;
	}
	return state;
};



Analyze the codebase context.
Identify the top 5-10 core most important abstractions to help those new to the codebase.

For each abstraction, provide:
1. A concise `name`.
2. A beginner-friendly `description` explaining what it is with a simple analogy, in around 100 words.
3. A list of relevant `file_indices` (integers) using the format `idx # path/comment`.

List of file indices and paths present in the context:
- 0 # README.md
- 1 # src/Actions.js
- 2 # src/index.js
- 3 # src/socket.js
- 4 # src/App.js
- 5 # src/components/navbar.js
- 6 # src/components/peditor.js
- 7 # src/components/footer.js
- 8 # src/components/pchat.js
- 9 # src/pages/NotFound.js
- 10 # src/pages/features.js
- 11 # src/pages/motivation.js
- 12 # src/pages/home.js
- 13 # src/pages/peerCall.js
- 14 # src/pages/addSlides.js
- 15 # src/pages/slides.js
- 16 # src/reducers/userReducer.js

Format the output as a YAML list of dictionaries:

```yaml
- name: |
    Query Processing
  description: |
    Explains what the abstraction does.
    It's like a central dispatcher routing requests.
  file_indices:
    - 0 # path/to/file1.py
    - 3 # path/to/related.py
- name: |
    Query Optimization
  description: |
    Another core concept, similar to a blueprint for objects.
  file_indices:
    - 5 # path/to/another.js
# ... up to 10 abstractions
```

