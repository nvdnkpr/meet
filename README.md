
# Meet

Provides a way to start multiple tasks with a single callback when all are finished.


## Install

	npm install meet


## Run tasks cuncurrently

	var Meet = require("meet")
	var meet = new Meet()

	meet.start( function() {
		var done = this
		// do stuff
		done()
	})

	meet.start( function() {
		var done = this
		// do other stuff
		done()
	})

	meet.allDone( function() {
		// all tasks have completed
	})


## Run tasks sequentially

	meet.queue( function() {
		var done = this
		// do something first 
		done()
	})

	meet.queue( function() {
		var done = this
		// do something after the first thing 
		done()
	})

	meet.allDone( function() {
		// all tasks have completed
	})



## Example


	var Meet = require("meet")
	var meet = new Meet()

	// These tasks are "started". They run concurrently & finish in random order
	function timeTask(t, id) {
		var done = this
		setTimeout(function() {
			console.log("time "+id+": "+t)
			done()
		}, t)
	}
	meet.start(timeTask, Math.random() * 6000, "a")
	meet.start(timeTask, Math.random() * 6000, "b")
	meet.start(timeTask, Math.random() * 6000, "c")
	meet.start(timeTask, Math.random() * 6000, "d")

	// These tasks are "queued".  They run sequentially in the order that they were queued
	function queueTask(t, id) {
		var done = this
		setTimeout(function() {
			console.log("queue "+id+": "+t)
			done()
		}, t)
	}
	meet.queue(queueTask, Math.random() * 2000, "A")
	meet.queue(queueTask, Math.random() * 2000, "B")
	meet.queue(queueTask, Math.random() * 2000, "C")
	meet.queue(queueTask, Math.random() * 2000, "D")

	// Note that the entire series of sequential tasks operates like a single concurrent
	// task relative to the other concurrent tasks.

	function finished(msg) {
		console.log(msg)
	}

	setTimeout(function() {
		console.log('seems like a nice time to set the completion call-back');
		meet.allDone(finished, "Everyone is done!")
	}, 4000);

