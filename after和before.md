<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
  <style>
    button{
      width:100px;
      border:1px solid black;
     border-radius:5px;
     position:relative;
    }

     button:before{
      content:attr(data-tip);
      opacity:0;
      position:absolute;
      top:200%;
      left:5%;
      background:#ffcb66;
      padding:5px 15px;
      white-space: nowrap;
      border-radius:10px;
      transition:all 0.4s ease;
      color:black;
    }

    button:after{

      content:'';
      width:0;
      height:0;
    /*  border-bottom: 10px solid #ffcb66;
         border-left: 10px solid transparent;
         border-right: 10px solid transparent; */
       border:8px solid transparent;
      border-bottom:8px solid #ffcb66;
      position:absolute;
      top:150%;
      left:50%;
      transition:all 0.4s ease;
      opacity:0;
    }

    button:hover:after{
       top:105%;
    }
    button:hover:before{
      top:180%;
    }
    button:hover:after, button:hover:before{
      opacity:1;
    }
    p:after{
      content:'';
     display:inline-block;
      border:8px solid transparent;
      border-bottom:8px solid red;
    }
    div{
      width:100px;
      height:80px;
      border:2px solid black;
      position:relative;
    }
     div:after{
      content:'';
      position:absolute;
      border:12px solid transparent;
      border-left:12px solid #fff;
      top:34%;
      left:100%
    }
    div:before{

      content:'';
      position:absolute;
      border:15px solid transparent;
      border-left:15px solid black;
      top:30%;
      left:100%
    }

  </style>
</head>
<body>


  <button  type="" value="确定" data-tip="这是一个提示">
    确定
  </button>
  <p>这是一个三角形</p>
  <div>
  </div>
</body>
</html>