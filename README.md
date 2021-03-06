# Hairy Text

Hairy Text is a tool for natural language processing. 

With Hairy Text, you can perform named entity recognition (NER) tasks using the
world-class [Spacy](https://spacy.io) library, and label data for training to
improve your model. All this from a normal looking and mobile-friendly web
application.

It is written in Elixir + Phoenix LiveView and Python, doesn't require a
database (totally self contained), and runs fine on a $5/mo server without a GPU.

# Screenshot

## List of examples for labeling
![HairyText Examples screenshot](https://i.imgur.com/2dvaxjx.png)

## Labeling interface in modal window
![HairyText Labeler screenshot](https://i.imgur.com/tWDeB6H.png)

## Testing unseen input (and adding to labeling queue)
![HairyText TEST screenshot](https://i.imgur.com/uXdzYx9.png)

# Features

* **Built with the awesome Spacy NLP framework** (so I probably didn't mess it up!)
* Easily label text fragments for machine learning / NLP experiments
* Interactive "test" console lets you quickly debug your model
* Refreshless but highly dynamic Phoenix Liveview web-based user interface (like React, without it)
* User logins with HTTP AUTH password check
* Export a .ZIP of your labeled examples and prediction history (both convenient .JSON files)
* REST JSON API for making predictions (to tie it into the rest of your project)
* API predictions stored in log and reviewable in app
* Label examples with a category
* Label text inside examples as entities - for instance "time reference" or "place name"
* Filter by entity tags or labels
* Train online via web interface and report live training progress (rough..)
* Support for multiple projects (some bugs)

# Future

* Make into embeddable component like LiveDashboard
* Support for "one at a time" editing that's more about a workflow of doing one labeling task after another
* Image tagging (including objects inside images)
* Assist in generating low-confidence predictions to more quickly improve model
* Each project should have its own DETS files
* APIs to: label examples new and old, bulk predict, view predictions log
* Learn from embeddings (BERT, I'm looking at you)
* uPlot training graphs

# Bugs

* Many Elixir warnings
* When creating a new example from the Predictions or Test screen, clicking on
the example text to label it will cause it to reset. This is really annoying.
Use a two-step editing process for now.
* Projects support broken

# Notes

Hairy Text uses a custom DETS-based storage system shim for training examples, logs, etc.
that integrates with Elixir's built in Ecto database framework (but only for
trivial parts of its functionality).

The connection to the Python-based NLP backend uses ErlPort

# Motivation

I wanted [Prodigy](https://prodi.gy/) but can't afford such bourgeoisie things.

# Requirements

* Elixir 1.10+
* Phoenix 1.5 + LiveView 
* Python 3.6+
* Spacy NLP toolkit for Python

# Installation

First, grab the code:

```
	$ git clone https://github.com/tlack/hairytext/
```

Then, install Spacy for Python. You'll need a recent version of Python. Consider using virtualenv with Hairytext. FYI, The Python code is in priv/python

```
	$ pip install spacy
```

Next, configure the default username/password:
```
	$ vim config/config.exs
```

Finally, install Elixir dependencies and start server:

```
	$ mix deps.get
	$ iex -S mix phx.server
```

Then open http://localhost:4141 to start playing. The default username and password is `admin`:`sohairy`

# API

```
$ curl 'http://localhost:4141/api/predict?text=i+am+live+on+twitch' | json_pp
{
	"text" : "i am live on twitch",
		"label" : "good",
		"label_confidence" : 0.999650120735168,
		"entities" : {
			"service" : "twitch"
		}
}
```

