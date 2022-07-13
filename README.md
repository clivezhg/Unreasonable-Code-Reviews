This special document might look a bit strange, it is mainly for explaining to the users what have happened to More-Lang, and some other people may find it useful to them (especially if having a hosted plugin, or interested in the ecosystem of Wordpress).

More-Lang was a welcoming Wordpress plugin before it was closed, it got the highest user review score (https://wordpress.org/support/plugin/more-lang/reviews/) in the hundreds of plugins which have significant users (i.e., >1000 installations) under the same category.

More-Lang was closed on 2021-7-9 without any warning. A few days later I consulted the team, it was said because my registered e-mail address failed to receive an automated mail, I needed to register a new email to re-open the plugin. After the new email was accepted by the system, I received code reviews from the [plugins@wordpress.org] for re-opening the plugin.

The reviews went on for a few rounds, however, from the points of me (who had been very familiar with the plugin), there's no benefit from the points of the reviewer, the recommended changes would even break the plugin logic and reduce the performance.

So, the reviews were totally unreasonable to me, the plugin was closed at last.

(P.S., there had been several rounds of reviews when I initially submitted the plugin to the [wordpress.org/plugins], then the plugin was accepted and hosted. However, all the points of the last reviewer had existed in those initial hosted files. So, either the last reviewer is unreasonable, or the first reviewer is unreasonable.)

(P.S., nearly all the points of the last reviewer also existed in the Wordpress core, but one of the reviewer's theories is like "something can be done by Wordpress core, but cannot be done by you". However, because More-Lang is a multilingual plugin, it will make multilingual equivalents of Wordpress core, so the reviewer's theory is not suitable for More-Lang. If it's a irreconcilable situtation, the initial reviewer should reject in the begining. So, as a whole the review team is untrustworthy & unpredictable).

The following are some key review points.

(1). Regarding this line: https://github.com/clivezhg/more-lang/blob/dfbe046a6136965e1ffa85ecb3ad63079b6eedde/ext/ml_custom.php#L115
```php
			add_post_meta( $object_id, $loc_meta_key, $_POST[$val_name] );
```
> The reviewer said '$_POST[$val_name]' (which is a post_meta translation) must be sanitized.
However, in the Wordpress Core when saving post_meta, it doesn't do any sanitizing: https://github.com/WordPress/WordPress/blob/2cc9f735238212adb0bba7888e11eccb4bf91bc2/wp-admin/includes/ajax-actions.php#L1627-L1649
<br>As said before, More-Lang is making multilingual equivalents of Wordpress Core, they must be the same formats. So any extra sanitizing than Wordpress Core is useless here, which may even break the business logic in some cases.
<br>(And like other More-Lang translations, More-Lang replace the Core post_meta value with the translated one after fetched from the DB using Wordpress filters ASAP, so it shall not introduce extra issue to Wordpress. I.e., the security is in the same level as Wordpress Core)


(2). Regarding this line: https://github.com/clivezhg/more-lang/blob/dfbe046a6136965e1ffa85ecb3ad63079b6eedde/inc/ml_pub.php#L209
```php
			$ml_requested_url_locale = trim( $_GET['lang'] );
```
> The reviewer said '$ml_requested_url_locale' must be sanitized.
Actually, in More-Lang, the '$ml_requested_url_locale' will be used to match a known set (e.g., it can only be "en" or "jp"), if it is not in the set, then "" will be assigned to '$ml_requested_url_locale'. This process is already a perfect sanitizing. Any extra sanitizing is nonsense, hard to understand, and will reduce the performance.
<br>The matching & assigning process is here: https://github.com/clivezhg/more-lang/blob/dfbe046a6136965e1ffa85ecb3ad63079b6eedde/inc/ml_pub.php#L221-L229


(3). Regarding this line (and many similiar lines): https://github.com/clivezhg/more-lang/blob/dfbe046a6136965e1ffa85ecb3ad63079b6eedde/inc/ml_editor_postinp.php#L17
```php
	<textarea id="content_<?php echo $mlocale; ?>" name="content_<?php echo $mlocale; ?>" class="ml-local-ta" style="display:none;"><?php
```
> The reviewer required "escape every variable" when 'echo'.
Firstly, in general sense, this guideline is incorrect. Because there's a very common coding pattern: store the html in a variable, change it, at last echo it, then 'esc...' will produce incorrect string.
<br>Secondly, there are many escaped 'echo's in More-Lang, and all unescaped are proved to be safe (i.e., their data sources are under controlled), to add 'esc...' to these safe 'echo's is useless and causing pefromance issue.
<br>Lastly, I inspected around 10 popular plugins, none of them does "escape every variable". Then I inspected Wordpress Core, it even does much worse than More-Lang, it doesn't 'esc...' some uncontrolled data.
<br>So, this guideline looks a little unrealistic (too awkward for most developers to implement), the reviewer was giving very very unfair task to More-Lang...but of course, the reviewer denied (without any reasonable explanation).

That's how More-Lan was closed. And as far as my inspection, More-Lang is safer than Wordpress Core & 95%+ of the hosted plugins, the reviewer closed it only because he/she desired to do, weaving variety of unreasonable reasons (otherwise 95%+ of the hosted plugins should be closed).

If you would like to comment this document or discuss, you can create issues in this project.
