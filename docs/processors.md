# Processors

Data processing is one of features deform.io provides. Processors are entities in a schema. They can have access to multiple properties.

Processors can be chained. 

There is also a processor dependency recursion check.

Feel free to send us a feedback about processors you are missing.

## Limitations

As for now processor cannot process properties of type **array**. In future we will implement this feature.

## Current processors implemented

Name                     | Description
-------------------------|------------
[apiai](/processors/#apiai)                    | Process natural language
[google_detect_language](/processors/#google_detect_language) | Detect the language of a text string
[google_translate](/processors/#google_translate)        | Translate text from one language to another language
[html_to_text](/processors/#html_to_text)           | Remove html tags from text
[markdown](/processors/#markdown)                 | Render a html from a markdown
[md5](/processors/#md5)                      | Calculate a message-digest fingerprint (checksum)
[random](/processors/#random)                   | Make a random value
[readability](/processors/#readability)              | Makes a text readable
[readability_com](/processors/#readability_com)         | Use readability.com to turn any web page into a clean view for reading
[resize](/processors/#resize)                   | Resize an image
[sentiment](/processors/#sentiment)                | Sentiment analysis aims to determine the attitude of a speaker or a writer with respect to some [topic or the overall contextual polarity of a text
[template](/processors/#template)                 | Make a string from template
[text_to_speech](/processors/#text_to_speech)         | Text to speech processor
[watermark](/processors/#watermark)                | Watermark an image
[yandex_detect_language](/processors/#yandex_detect_language) | Detect the language of a text string
[yandex_speechkit_tts](/processors/#yandex_speechkit_tts) | Text to speech processor
[yandex_translate](/processors/#yandex_translate)        | Translate text from one language to another language

    
## Examples

### apiai

Process natural language

	{
	    "type": "object",
	    "properties": {
	    	"human_text": {
        		"type": "string",
        		"required": true
        	},
            "ai_obj": {
            	"type": ["object", "null"],
            	"additionalProperties": true,
            	"processors": [
            		{
            			"name": "apiai",
            			"in": {
							"text": {
								"property": "human_text"
							},
							"language": {
								"value": "EN"
							},
							"language_token": {
								"value": "<YOUR_LANGUAGE_TOKEN>"
							},
							"subscription_key": {
								"value": "<YOUR_SUBSCTIPTION_KEY>"	
							},
							"timezone": {
								"value": "Europe/Moscow"
							},
							"contexts": {
								"value": ["calendar"]
							}
						}
            		}
            	]
            }
	    }
	}


Request body:

	{
		"human_text": "schedule lunch at 1 pm tomorrow.",
	}

Response:

	{
        "_id": "572b676a32d2c61886cda77b",
        "ai_obj": {
            "action": "calendar.add",
            "metadata": {
                "contexts": [],
                "inputContexts": [],
                "outputContexts": []
            },
            "parameters": {
                "endDate": "2016-05-06T13:00:00+03:00",
                "startDate": "2016-05-06T13:00:00+03:00",
                "summary_predefined": "lunch"
            },
            "resolvedQuery": "schedule lunch at 1 pm tomorrow.",
            "score": 0,
            "source": "domains",
            "speech": ""
        },
        "human_text": "schedule lunch at 1 pm tomorrow."
    }




### google_detect_language

Detect the language of a text string

	{
	    "type": "object",
	    "properties": {
	    	"text": {
        		"type": "string"
        	},
        	"detected": {
        		"type": "string",
        		"processors": [
        			{
        				"name": "google_detect_language",
        				"in": {
                            "text": {
                                "property": "text"
                            },
                            "api_key": {
                                "value": "<YOUR_API_KEY>"
                            }
                        }
        			}
        		]
        	}
	    }
	}

Request body:
	
	{
		"text": "Привет"
	}

Response:

	{
    
        "_id": "572a45ed32d2c602098508e0",
        "text": "Привет",
        "detected": "ru"
    }


### google_translate

Translate text from one language to another language

	{
	    "type": "object",
	    "properties": {
	    	"text": {
        		"type": "string"
        	},
        	"translated": {
        		"type": "string",
        		"processors": [
        			{
        				"name": "google_translate",
        				"in": {
                            "text": {
                                "property": "text"
                            },
                            "from": {
                                "value": "ru"
                            },
							"to": {
                                "value": "en"
                            },
							"api_key": {
                                "value": "<YOUR_API_KEY>"
                            }
                        }
        			}
        		]
        	}
	    }
	}

Request body:
	
	{
		"text": "Привет"
	}

Response:

	{
        "_id": "572a45ec32d2c602098508d5",
        "text": "Привет",
        "translated": "Hi"
	}


### html_to_text

Remove html tags from text

	{
	    "type": "object",
	    "properties": {
	    	"html": {
        		"type": "string"
        	},
        	"text": {
        		"type": "string",
        		"processors": [
        			{
        				"name": "html_to_text",
        				"in": {
                            "html": {
                                "property": "html"
                            }
                        }
        			}
        		]
        	}
	    }
	}

Request body:
	
	{
		"html": "<html><head><title>Hello</title></head><body>World</body></html>"
	}

Response:

	{
        "_id": "572b75f632d2c620e67a8908",
        "html": "<html><head><title>Hello</title></head><body>World</body></html>",
        "text": "World"
	}


### markdown

Render a html from a markdown

	{
	    "type": "object",
	    "properties": {
	    	"markdown": {
				"type": "string",
				"required": true
			},
			"html": {
				"type": "string",
				"description": "rendered markdown",
				"processors": [
					{
						"name": "markdown",
						"in": {
							"text": {
								"property": "markdown"
							}
						}
					}
				]
			}
	    }
	}

Request body:

	{
		"markdown": "**hello** __world__"
	}

Response:
	
	{
        "_id": "572b746a32d2c61f0a366d87",
        "html": "<p><strong>hello</strong> <strong>world</strong></p>\n",
        "markdown": "**hello** __world__"
	}


### md5

Calculate a message-digest fingerprint (checksum)

	{
	    "type": "object",
	    "properties": {
	    	"firstName": {
				"type": "string"
			},
			"lastName": {
				"type": "string"
			},
			"password_md5ed": {
				"type": "string",
				"description": "user password",
				"processors": [
					{
						"name": "md5"
					}
				],
				"required": true
			}
	    }
	}

Request body:
	
	{
		"firstName": "Andrey",
		"lastName": "Chibisov",
		"password": "1234567890"
	}

Response:

	{
        "_id": "jKmLOGOunnNrLe",
        "firstName": "Andrey",
        "lastName": "Chibisov",
        "password_md5ed": "e807f1fcf82d132f9bb018ca6738a19f"
	}


### random

Make a random value

	{
	    "type": "object",
	    "properties": {
	    "firstName": {
				"type": "string"
			},
			"lastName": {
				"type": "string"
			},
			"password": {
				"type": "string",
				"description": "random user password",
				"processors": [
					{
						"name": "random",
						"in": {
							"random_type": {
								"value": "string"
							},
							"length": {
								"value": 20
							}
						}
					},
					{
						"name": "md5"
					}
				]
			}
		},
		"required": ["firstName", "lastName"]
	}


Request body:
	
	{
		"firstName": "Gennady",
		"lastName":  "Chibisov"
	}

Response:
	

	{
        "_id": "572b722a32d2c61d34331ef0",
        "firstName": "Gennady",
        "lastName": "Chibisov",
        "password": "1591bbef3c4792bf9e102b10cad32b1c"
	}

### readability

Makes a text readable
	
	{
	    "type": "object",
	    "properties": {
	    	{
		"type": "object",
		"properties": {
			"html_content": {
				"type": "string"
			},
			"readable": {
				"type": "object",
				"properties": {
					"content": {
						"type": "string"
					},
					"short_title": {
						"type": "string"
					},
					"title": {
						"type": "string"
					}
				},
				"processors": [
					{
						"name": "readability",
						"in": {
							"text": {
								"property": "html_content"
							}
						}
					}
				]
			}
	    }
	}

Request body:

	{
		"html_content": "<html><head><title>Hello</title></head><body>World</body></html>",
	}

Response:

	{
        "_id": "572b70b932d2c61c27a112bb",
        "html_content": "<html><head><title>Hello</title></head><body>World</body></html>",
        "plaintext": "World",
        "readable": {
            "content": "<body id=\"readabilityBody\">World</body>",
            "short_title": "Hello",
            "title": "Hello"
        }
	}



### readability_com

Use [readability.com](https://readability.com/) to turn any web page into a clean view for reading

	{
	    "type": "object",
	    "properties": {
	    	"url_to_make_readable": {
        		"type": "string"
        	},
        	"readable": {
        		"type": "object",
        		"additionalProperties": true,
        		"processors": [
        			{
        				"name": "readability_com",
        				"in": {
                            "url": {
                                "property": "url_to_make_readable"
                            },
							"api_key": {
                                "value": "<YOUR_API_KEY>"
                            }
                        }
        			}
        		]
        	}
	    }
	}

Request body:
	
	{
		"url_to_make_readable": "https://blog.chib.me/how-to-convert-databases-with-one-line-of-code/",
	}


Response:

	{
        "_id": "572b7cb032d2c628585e1fa9",
        "readable": {
            "author": null,
            "content": "............",
            "date_published": "2016-02-07 13:03:06",
            "dek": null,
            "direction": "ltr",
            "domain": "blog.chib.me",
            "excerpt": "07 February 2016 Have you ever wanted to convert mysql database to sqlite? Or postgres to mysql? Or mysql to postgres? Recently I've been migrating my django project from MySQL to SQLite. I tried to&hellip;",
            "lead_image_url": null,
            "next_page_id": null,
            "rendered_pages": 1,
            "short_url": "http://rdd.me/7gou967w",
            "title": "How to convert databases with one line of code",
            "total_pages": 0,
            "url": "https://blog.chib.me/how-to-convert-databases-with-one-line-of-code/",
            "word_count": 421
        },
        "url_to_make_readable": "https://blog.chib.me/how-to-convert-databases-with-one-line-of-code/"
	}


### resize

Resize an image

	{
	    "type": "object",
	    "properties": {
	    	"original": {
				"type": "file"
			},
			"resized": {
				"type": "file",
				"processors": [
					{
						"name": "resize",
						"in": {
					        "original_image": {
					            "property": "original"
					       	},
							"size": {
								"value": [200,200]
							}
						}
					}
				]
			}
	    }
	}

Commit a `PUT`/`POST`/`PATCH` with `Content-Type: multipart/form-data` and file assigned to `original` property

Response:

	{
        "_id": "572b6e7732d2c61b066cee6e",
        "original": {
            "_id": "572b6e7732d2c61b066cee6f",
            "collection_id": "QSMHdGfXIQIXOv",
            "content_type": "image/gif",
            "date_created": "2016-05-05T16:01:59.614410146Z",
            "document_id": "572b6e7732d2c61b066cee6e",
            "last_access": "2016-05-05T16:01:59.614410146Z",
            "md5": "8c4c6953f0b9003eb9ceb7a923cc208e",
            "name": "test_helpers/data/animated.gif",
            "size": 991109
        },
        "resized": {
            "_id": "572b6e7832d2c61b066cee71",
            "collection_id": "QSMHdGfXIQIXOv",
            "content_type": "image/gif",
            "date_created": "2016-05-05T16:02:00.475956835Z",
            "document_id": "572b6e7732d2c61b066cee6e",
            "last_access": "2016-05-05T16:02:00.475956835Z",
            "md5": "d741fa8da603842a0dd0aec9ec2330df",
            "name": "resized_resize_OLYi.gif",
            "size": 366404
        }
	}


### sentiment

Sentiment analysis aims to determine the attitude of a speaker or a writer with respect to some topic or the overall contextual polarity of a text

	{
	    "type": "object",
	    "properties": {
	    	"text": {
				"type": "string"
			},
			"sentiment": {
				"type": "number",
				"description": "text to sentiment",
				"processors": [
					{
						"name": "sentiment",
						"in": {
							"text": {
								"property": "text"
							}
						}
					}
				]
			}
	    }
	}

Request body:

	{
		"text": "this restaurant is great",
	}

Response:

	{
        "_id": "572b6d4132d2c61a78927e5d",
        "sentiment": 0.8,
        "text": "this restaurant is great"
	}


### template

Make a string from template

	{
	    "type": "object",
	    "properties": {
			"country": {
				"type": "object",
				"properties": {
					"name": {
						"type": "string",
						"required": true
					},
					"code": {
						"type": "string",
						"required": true
					}
				}
			},
			"representation": {
				"type": "string",
				"processors": [
					{
						"name": "template",
						"in": {
                        	"context": {
                        		"code": {
                        			"property": "country.code"
                        		},
                        		"name": {
                        			"property": "country.name"
                        		}
                        	},
                            "syntax": {
                            	"value": "handlebars"
                            },
                            "template_string": {
                            	"value": "{{code}} - {{name}}"
                        	}
                    	}
					}
				]
			}
		}
	}

Request body:

    {
    	"country": {
    		"name": "Russian Federation",
    		"code": "RU"
    	}
    }

Response:

	{
		"_id": "562b6b7a32e3c118e83aa4fa",
		"country": {
    		"name": "Russian Federation",
    		"code": "RU"
    	},
    	"representation": "RU - Russian Federation"
	}


### text_to_speech

Text to speech processor

	{
	    "type": "object",
	    "properties": {
			"user_info": {
				"type": "object",
				"properties": {
					"firstName": {
						"type": "string"
					},
					"lastName": {
						"type": "string"
					},
					"age": {
						"description": "Age in years",
						"type": "integer",
						"minimum": 0
					}
				}
			},
			"message": {
				"type": "string",
				"processors": [
					{
						"name": "template",
						"in": {
							"context": {
                        		"firstName": {
                        			"property": "user_info.firstName"
                        		},
                        		"lastName": {
                        			"property": "user_info.lastName"
                        		},
                        		"age": {
                        			"property": "user_info.age"
                        		}
                        	},
							"syntax": {
								"value":"handlebars"
							},
                            "template_string": {
                            	"value":"{{lastName}} {{firstName}} is {{age}} years"
                            }
						}
					}
				]
			},
			"tts_field": {
				"type": "file",
				"processors": [
					{
						"name": "text_to_speech",
						"in": {
							"text": {
								"property": "message"
							},
							"language": {
								"value": "en-US"
							}
						}
					}
				]
			}
		}
	}


Request body:

	{
        "user_info": {
            "lastName":  "Chibisov",
            "firstName": "Andrey",
            "age":       27
		}
	}

Response:

	{
        "_id": "572b6b7832d2c619e83aa4fd",
        "message": "Chibisov Andrey is 27 years",
        "tts_field": {
            "_id": "572b6b7a32d2c619e83aa4fe",
            "collection_id": "example_schema_tts-MuVVGXLnShseQO",
            "content_type": "audio/x-wav",
            "date_created": "2016-05-05T15:49:14.342821384Z",
            "document_id": "572b6b7832d2c619e83aa4fd",
            "last_access": "2016-05-05T15:49:14.342821384Z",
            "md5": "8a2bab3a7b3752028e9dfbfa326d5e73",
            "name": "tts_field_text_to_speech_trde.wav",
            "size": 59206
        },
        "user_info": {
            "age": 27,
            "firstName": "Andrey",
            "lastName": "Chibisov"
        }
	}


### watermark

Watermark an image

	{
	    "type": "object",
	    "properties": {
	        "watermark": {
	            "type": "file"
	        },
	        "photo": {
	            "type": "file",
	            "processors": [
	                {
	                    "name": "resize",
	                    "in": {
	                        "size": {
	                            "value": [
	                                100,
	                                100
	                            ]
	                        }
	                    }
	                },
	                {
	                    "name": "watermark",
	                    "in": {
	                        "watermark_image": {
	                            "property": "watermark"
	                        },
	                        "gravity": {
	                            "value": "SouthEast"
	                        }
	                    }
	                }
	            ]
	        }
	    }
	}


Request body:
	
	{
		"photo": "<YOUR_IMAGE_FILE>",
		"watermark": "<YOUR_WATERMARK_FILE>"
	}
	

### yandex_detect_language

Detect the language of a text string

	{
	    "type": "object",
	    "properties": {
	    	"text": {
        		"type": "string"
        	},
        	"detected": {
        		"type": "string",
        		"processors": [
        			{
        				"name": "yandex_detect_language",
        				"in": {
                            "text": {
                                "property": "text"
                            },
							"api_key": {
                                "value": "<YOUR_API_KEY>"
                            }
                        }
        			}
        		]
        	}
	    }
	}

Request body:

	{
		"text": "Hello world",
	}

Response:

	{
        "_id": "572b7a9432d2c6241b8bbe22",
        "text": "Hello world",
        "detected": "en"
	}


### yandex_speechkit_tts

Text to speech processor

	{
		"type": "object",
		"properties": {
			"text_to_speak": {
				"type": "string"
			},
			"speech": {
				"type": "file",
				"processors": [
					{
						"name": "yandex_speechkit_tts",
						"in": {
							"text": {
								"property": "text_to_speak"
							},
							"format": {
								"value": "wav"
							},
							"language": {
								"value": "ru-RU"
							},
							"speaker": {
								"value": "omazh"
							},
							"api_key": {
								"value": "<YOUR_API_KEY>"
							},
							"emotion": {
								"value": "good"
							}
						}
					}
				]
			}
		}
	}

Request body:
	
	{
		"text_to_speak": "Как дела?"
	}

Response:
	
	{
        "_id": "572b791532d2c6226354dd26",
        "text_to_speak": "Как дела?",
        "speech": {
            "_id": "572b6b7a32d2c619e83aa4fe",
            "collection_id": "example_schema_tts-aA8rCg9UeRQVkm",
            "content_type": "audio/x-wav",
            "date_created": "2016-06-17T15:30:37.496648327Z",
            "document_id": "572b6b7832d2c619e83aa4fd",
            "last_access": "2016-06-17T13:30:37.496648327~",
            "md5": "8a2bab3a7b3752028e9dfbfa326d5e73",
            "name": "tts_field_text_to_speech_trde.wav",
            "size": 59206
        }
	}

### yandex_translate

Translate text from one language to another language

	{
	    "type": "object",
	    "properties": {
	    	"text": {
        		"type": "string"
        	},
        	"translated": {
        		"type": "string",
        		"processors": [
        			{
        				"name": "yandex_translate",
        				"in": {
                            "text": {
                                "property": "text"
                            },
                            "lang": {
                                "value": "ru-en"
                            },
							"api_key": {
                                "value": "<YOUR_API_KEY>"
                            }
                        }
        			}
        		]
        	}
	    }
	}

Request body:
	
	{
		"text": "Как дела?"
	}

Response:
	
	{
        "_id": "572b791532d2c6226354dd26",
        "text": "Как дела?",
        "translated": "How's it going?"
	}

