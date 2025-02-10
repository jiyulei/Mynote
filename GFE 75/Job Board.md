#gfe75 
	1.`fetch`的用法，[[Http & API]]
	2.`timestamp` 转换
	3. ```setJobDetails(prev => [...prev, ...newJobDetails]);
      setCurIndex(prev => prev + size);```
       确保state已经更新

``` javascript
import { useState, useEffect } from 'react';

function ListItem ({ by, title, time, url}) {
  return <div className='listItem'>
    <div>
      <h3><a href={url}>{title}</a></h3>
    </div>
    <div>
      By {by} {formatTimeStamp(time)}
    </div>
  </div>
}

function formatTimeStamp(time) {
  // timestamp转换
  const timestamp = time * 1000;
  const date = new Date(timestamp);
  return date.toLocaleDateString('en-US', {
    month: 'numeric',
    day: 'numeric',
    year: 'numeric',
    hour: 'numeric',
    second: '2-digit',
    minute: '2-digit',
    hour12: true,
  });
}

export default function App() {
  const [jobIds, setJobIds] = useState([]);
  const [jobDetails, setJobDetails] = useState([]);
  const [curIndex, setCurIndex] = useState(0);
  const size = 6;

   async function fetchJobDetails(jobid) {
      try {
        const response = await fetch(`https://hacker-news.firebaseio.com/v0/item/${jobid}.json`, {
          method: 'GET',
          headers: {'Content-Type': 'application/json'}
        });
        const data = await response.json();
        return data;
      } catch(error) {
        console.error(error);
      }
    }

  useEffect(() => {
    async function fetchJobIds() {
      try {
        const response = await fetch(`https://hacker-news.firebaseio.com/v0/jobstories.json`, {
          method: 'GET',
          headers: {'Content-Type': 'application/json'}
        });

        const jobIds = await response.json();
        setJobIds(jobIds);
      } catch(error) {
        console.error(error);
      }
    }

    fetchJobIds();
  }, [])

  useEffect(() => {
    async function loadInitialJob () {
      const jobDetails = await Promise.all(
      jobIds.slice(0, 6).map(id => fetchJobDetails(id)));
      setJobDetails(jobDetails);
    }
    loadInitialJob();
    setCurIndex(6);
  }, [jobIds])

  const handleClick = () => {
    async function loadMoreJobs() {
      const newJobDetails = await Promise
      .all(jobIds.slice(curIndex, curIndex + size)
      .map(id => fetchJobDetails(id)));
  

      setJobDetails(prev => [...prev, ...newJobDetails]);
      setCurIndex(prev => prev + size);
    }
    loadMoreJobs();
  }

   return (
    <div>
      <h1 style={{color: 'orangeRed'}}>Hacker News Jobs Board</h1>
      <div> 
       {jobDetails.map((item) => 
       (<ListItem by={item.by} title={item.title} time={item.time} url={item.url}/>))}
      </div>

      <button onClick={handleClick}>Load more jobs</button>
    </div>
  );
}
```

``` css
body {
  font-family: sans-serif;
}

button {
  color: white;
  background-color: orangered;
  padding: 10px 10px;
  border-radius: 5px;
  border: 1px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: orange;
}

.listItem {
  box-shadow: 1px 1px 3px gray;
  border: 1px solid rgba(128, 128, 128, 0.366);
  border-radius: 4px;
  margin-bottom: 5px;
  padding: 10px ;
}

h3 a {
  text-decoration: none;
  color: inherit;
}

h3 a:hover {
   text-decoration: underline;
}
```