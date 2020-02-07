Extended carbs / "eCarbs"
**************************************************
With a regular pump therapy, extended boluses are a good way to deal with fatty or otherwise slowly-absorbed meals which increase blood glucose longer than the insulin is in effect. In a loop context, however, extended boluses don't make as much sense (and pose technical difficulties), since they're basically a fixed high temporary basal rate, which goes against how the loop works, which is adjusting the basal rate dynamically. For details see `extended bolus <../Usage/Extended-Carbs.html#extended-bolus>`_ below.

The need to deal with such meals still exists though. Which is why AndroidAPS as of version 2.0 supports so called extended carbs or eCarbs.

eCarbs are carbs that are spilt up over several hours. For standard meals with more carbohydrates than fat/protein, entering the carbs up front (and reducing the initial bolus if needed) is usually sufficient to prevent too-early insulin delivery.  But for slower-absorbing meals where full carb entry up front results in too much IOB from SMB, eCarbs can be used to more accurately simulate how the carbs (and any carb equivalents you enter for other macronutrients) are absorbed and influence the blood glucose. With this information, the loop can administer SMBs more gradually to deal with those carbs, which can be seen as a dynamic extended bolus (this should also work without SMBs, but is probably less effective).

eCarbs aren't limited to fatty / protein heavy meals: they can be also be used to help in any situation where there are influences that increase the blood sugar, e.g. other medication like corticosteroids.

To enter eCarbs, set a duration in the _Carbs_ dialog on the overview tab, the total carbs and optionally a time shift:

.. image:: ../images/eCarbs_Dialog.png
  :alt: Enter carbs

The eCarbs on the overview tab, note the carbs in brackets at the COB field, which shows the carbs in the future:

.. image:: ../images/eCarbs_Graph.png
  :alt: eCarbs in graph

Carb entries which are in the future are coloured in dark orange on the treatment tab:

.. image:: ../images/eCarbs_Treatment.png
  :alt: eCarbs in future in treatment tab


-----

A way to handle fat and protein with that feature is described here: `https://adriansloop.blogspot.co.at/2018/04/page-margin-0.html <https://adriansloop.blogspot.co.at/2018/04/page-margin-0.html>`_

-----

The recommended setup is to use the OpenAPS SMB APS plugin, with SMBs enabled as well as the _Enable SMB with COB_ preference being enabled.

A scenario e.g. for a Pizza might be to give a (partial) bolus up front via the _calculator_ and then use the _carbs_ button to enter the remaining carbs for a duration of 4-6 hours, starting after 1 or 2 hours. You'll need to try out and see which concrete values work for you of course. You might also carefully adjust the setting _max minutes of basal to limit SMB to_ to make the algorithm more or less aggressive.
With low carb, high fat/protein meals it may be enough to only use eCarbs without manual boluses (see the blog post above).

When eCarbs are generated, a Careportal note is also created to document all inputs, to make it easier to iterate and improve inputs.

Extended bolus
==================================================

Why extended bolus does not really work in a closed loop environment
-----------------------------------------------------------------------
As mentioned above extended or multiwave boluses do not really work in a closed loop environment. Here's why:

1. The loop determines that now 1.55U/h is to be delivered. Whether this is delivered as an extended bolus or TBR does not matter to the algorithm. In fact, some of the pumps use the extended bolus. What should happen then? Most pump drivers then stop the extended bolus -> You didn't even need to start it.
2. If you had the extended bolus as input, what should happen in the model?

   1. Should it be considered neutral together with the BR and looped on it? Then the loop should also be able to reduce the bolus if, for example, you get too low and all the "neutral" insulin is taken away?
   2. Should the extended bolus simply be added? So the loop should simply be allowed to continue? Even in the worst hypo? I don't think this is so good: A hypo is foreseen but it must not be prevented?
   
3. The IOB that the extended bolus builds up materializes after 5 minutes at the next run. Accordingly, the loop would give less basal. So not much changes... except that the possibility of hypo avoidance is taken.

Extended bolus option in AAPS
-----------------------------------------------------------------------
As a lot of new loopers have asked for this funcunality as of version 2.6 there will be the option to use extended bolus in AndroidAPS. **To prevent problems closed loop will be disabled for the duration of the extended bolus.**

1. Open Action tab (ACT)
2. Select extended bolus
3. Confirm that **closed loop mode will be stopped** for the time of running extended bolus.
4. On the homescreen you will find information about inuslin units and remaining time of the extended bolus as well as the warning that loop is disabled.

.. image:: ../images/ExtendedBolus_V2_6.png
   :alt: Extended bolus
