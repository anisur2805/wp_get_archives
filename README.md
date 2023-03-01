# wp_get_archives
### This by default prevent the `wp_get_archives()` method redirect to new page and show the result on the bottom of the dropdown with `ajax` call.

```
<div id="archive-list">
    <select name="archive-dropdown">
        <option value=""><?php esc_attr(_e('Select Month', 'textdomain')); ?></option>
        <?php wp_get_archives( array(
            'type'              => 'yearly', 
            'format'            => 'option', 
            'show_post_count'   => 1,
            'post_type'         => 'post',
        )); ?>
    </select>

    <div id="loader" style="display:none;">
        Loading...
    </div>
</div>
<div class="append-content"></div>

<script>
    jQuery(document).ready(function($) {
        $('#archive-list select').change(function(e) {
            e.preventDefault();

            $('#loader').show();
            var link = $(this)[0].value;

            $.ajax({
                url: link,
                type: 'GET',
                success: function(data) {
                    $('#loader').hide();
                    var archiveContent = $(data).find('.main-content > .row > .post-area').html();
                    $('.append-content').html(archiveContent);
                }
            });
        });
    });
</script>
```

1. Type can be ``` 'daily', 'weekly', 'monthly', 'yearly', 'postbypost', or 'alpha'```
2. ``` show_post_count ``` can be `1` or `0` - will show post count beside post option
3. ``` post_type ``` your custom post type or default post
4. Show a loader before the post is display
5. Append the results inside ``` append-content ``` `div`
6. Make the `ajax` call to fetch and append the result.
