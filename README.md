> :warning: This software is not of high quality, is incomplete and does not respond well to errors or misconfiguration. I've done my best to make it work and do what I need - label printing with "grocy". But for that I had to mess around in python code without any experience with the programming language.


## brother\_ql\_web

This is a web service to print labels on Brother QL label printers.

You need Python 3 for this software to work.

![Screenshot](./static/images/screenshots/Label-Designer_Desktop.png)

The web interface is [responsive](https://en.wikipedia.org/wiki/Responsive_web_design).
There's also a screenshot showing [how it looks on a smartphone](./static/images/screenshots/Label-Designer_Phone.png)

### Installation

**ProTip™**: If you know how to use Docker, you might want to use my ready-to-use Docker image to deploy this software.
It can be found [on the Docker hub](https://hub.docker.com/r/pklaus/brother_ql_web/).  
> :warning: [koppenho](https://github.com/koppenho): The mentioned docker image did not work for me. It took me about a day or two to figure out the root cause.
> After I have finished work on this fork, I plan to publish a new docker image.

For manual installation, follow the instructions below.

Get the code:

    git clone https://github.com/pklaus/brother_ql_web.git

or download [the ZIP file](https://github.com/pklaus/brother_ql_web/archive/master.zip) and unpack it.

Install the requirements:

    pip install -r requirements.txt

In addition, `fontconfig` should be installed on your system. It's used to identify and
inspect fonts on your machine. This package is pre-installed on many Linux distributions.
If you're using a Mac, I recommend to use [Homebrew](https://brew.sh) to install
fontconfig using [`brew install fontconfig`](http://brewformulas.org/Fontconfig).

### Configuration file

Copy `config.example.json` to `config.json` (e.g. `cp config.example.json config.json`) and adjust the values to match your needs.

### Startup

To start the server, run `./brother_ql_web.py`. The command line parameters overwrite the values configured in `config.json`. Here's its command line interface:

    usage: brother_ql_web.py [-h] [--port PORT] [--loglevel LOGLEVEL]
                             [--font-folder FONT_FOLDER]
                             [--default-label-size DEFAULT_LABEL_SIZE]
                             [--default-orientation {standard,rotated}]
                             [--model {QL-500,QL-550,QL-560,QL-570,QL-580N,QL-650TD,QL-700,QL-710W,QL-720NW,QL-1050,QL-1060N}]
                             [printer]
    
    This is a web service to print labels on Brother QL label printers.
    
    positional arguments:
      printer               String descriptor for the printer to use (like
                            tcp://192.168.0.23:9100 or file:///dev/usb/lp0)
    
    optional arguments:
      -h, --help            show this help message and exit
      --port PORT
      --loglevel LOGLEVEL
      --font-folder FONT_FOLDER
                            folder for additional .ttf/.otf fonts
      --default-label-size DEFAULT_LABEL_SIZE
                            Label size inserted in your printer. Defaults to 62.
      --default-orientation {standard,rotated}
                            Label orientation, defaults to "standard". To turn
                            your text by 90°, state "rotated".
      --model {QL-500,QL-550,QL-560,QL-570,QL-580N,QL-650TD,QL-700,QL-710W,QL-720NW,QL-1050,QL-1060N}
                            The model of your printer (default: QL-500)

### Usage

Once it's running, access the web interface by opening the page with your browser.
If you run it on your local machine, go to <http://localhost:8013> (You can change
the default port 8013 using the --port argument).
You will then be forwarded by default to the interactive web gui located at `/labeldesigner`.

All in all, the web server offers:

* a Web GUI allowing you to print your labels at `/labeldesigner`,
* an API at `/api/print/text?text=Your_Text&font_size=100&font_family=DejaVu%20Sans%20(Book)`
  to print a label containing 'Your Text' with the specified font properties.
* You may test grocy label printing without actually printing. The following command will save an image of the generated and to-be-printed label in file `image.png`.

  `curl -S -o image.png 'http://localhost:8013/api/preview/grocy?product=Applejuice&grocycode=grcy:p:4711:63ad991cdc1a9&font_family=DejaVu%20Sans%20(Book)&due_date=2099-12-31'`

### Grocy configuration

I use the following parameters in grocy's config.php file:

    // The URI that grocy will POST to when asked to print a label
    // You may have to replace 'localhost' by a hostname or IP address
    Setting('LABEL_PRINTER_WEBHOOK', 'http://localhost:8013/api/print/grocy');
    // Additional parameters supplied to the webhook
    // Currently 'font_family' must be set to a known font name
    Setting('LABEL_PRINTER_PARAMS', ['font_family' => 'DejaVu Sans (Book)']);

### License

This software is published under the terms of the GPLv3, see the LICENSE file in the repository.

Parts of this package are redistributed software products from 3rd parties. They are subject to different licenses:

* [Bootstrap](https://github.com/twbs/bootstrap), MIT License
* [Glyphicons](https://getbootstrap.com/docs/3.3/components/#glyphicons), MIT License (as part of Bootstrap 3.3)
* [jQuery](https://github.com/jquery/jquery), MIT License
