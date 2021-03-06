# Quotes for October

An OctoberCMS default plugin for adding testimonial quotes to your website, including custom avatars.

## Install

Add as a submodule to your project:

```bash
$ git submodule add git@github.com:swt83/php-october-quotes.git plugins/travis/quotes
```

## Usage

Build a partial for use in your pages:

```
<?php
use Travis\Quotes\Models\Quote;

function onStart()
{
    // cache forever
    $this['quotes'] = Cache::rememberForever('quotes', function()
    {
        // load
        $records = Quote::orderBy('sort_order', 'asc')->get();

        // init
        $clean = [];

        // foreach record...
        foreach ($records as $record)
        {
            // convert to array
            $array = $record->toArray();

            // capture avatar path
            $array['avatar'] = is_object($record->avatar) ? $record->avatar->path : null;

            // save
            $clean[] = $array;
        }

        // return
        return $clean;
    });
}
?>
==
<div id="quotes">
    {% for quote in quotes %}
        <div class="quote">
            <div class="photo"><img src="{{ quote.avatar.path }}" alt="" /></div>
            <p>&ldquo;{{ quote.quote }}&rdquo;</p>
            <div class="author">
                <div class="author_name">{{ quote.author_name }}</div>
                <div class="author_title">{{ quote.author_title }}, {{ quote.author_organization }}</div>
            </div>
        </div>
    {% endfor %}
</div>
```
