const express = require('express')
const path = require('path')
const parser = require('body-parser');
const PORT = process.env.PORT || 5000
const { Pool } = require('pg')
    
const connectionString = process.env.DATABASE_URL || 'postgres://fyhdputyskavyj:77b22da05e0a4cb42cf9862010d38b7f584377986d995b57f48d8da1cb82b294@ec2-107-22-211-248.compute-1.amazonaws.com:5432/dcrv4m1b2g60k1?ssl=true';

const pool = new Pool({connectionString: connectionString});

var sql = "SELECT * FROM progress";

express()
  .use(express.static(path.join(__dirname, 'public')))
    .use(parser.urlencoded({ extended: false }))
    .use(parser.json())

  .set('views', path.join(__dirname, 'views'))
  .set('view engine', 'ejs')
    .get('/', (req, res) => res.render('pages/login'))
    .post('/progress', function(req, res) {
	    sql = sql + " WHERE name = '" + req.body.name + "'";
	    pool.query(sql, function(err, result) {
		    if (err) {
			console.log("Error in query: ")
			    console.log(err);
		    }
		    console.log("Back from DB with result:");
		    console.log(result.rows);
		    var data = {
			name : req.body.name,
			task : getTasks(result.rowCount, result.rows),
			size : result.rowCount,
			object : result.rows
		    };
		    res.render('pages/progress', {
			    userValue : data
				
				});
		});
	}
	)
    .get('/data/:data', function(req, res){
	    var dataNew = req.params.data;
	    var taskNew = "DELETE FROM progress WHERE studentid = " + dataNew;
	    console.log(taskNew);
	    var get = "SELECT name FROM progress WHERE studentid = " + dataNew;
	    pool.query(get, function(err, result) {
		    if (err) {
                        console.log("Error in query: ")
                            console.log(err);
                    }
		    var name = result.rows[0].name;

		    console.log(name);
		    pool.query("SELECT * FROM progress WHERE name ='" + name + "'", function(err, result) {
			    if (err) {
				console.log("Error in query: ")
				    console.log(err);
			    }
			    console.log("Back from DB with result:");
			    console.log(dataNew);
			    var data = {
				task : getTasks(result.rowCount, result.rows),
				size : result.rowCount,
				object : result.rows,
				skip : dataNew
			    };
			    res.render('pages/data', {
				    userValue : data

					});

			});
		    pool.query(taskNew, function(err, result){
                            if (err) {
                                console.log("Error in query: ")
                                    console.log(err);
                            }
                            console.log(result);
                            console.log("Successfully Deleted");
                        });
		});	    
	})
    .get('/add/:name', function(req, res){
	    var name = {
		name : req.params.name
	    };
	    res.render('pages/add', {
		name : name
			});
	})
    .get('/addTask/:json', function(req, res){
	    var info = req.params.json;
	})
    .listen(PORT, () => console.log(`Listening on ${ PORT }`))


    function getTasks(size, data) {
    var task = new Array();
    for (var i = 0; i < size; i++)
	{
	    task[i] = data[i].task + " " + (data[i].duedate.getMonth() + 1) + "/" + data[i].duedate.getDate() + "/" + data[i].duedate.getFullYear();
	}
    return task;
    }
