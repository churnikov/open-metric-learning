How to make inference and extract features
------------------------------------------
OML can do inference on your data using trained model. As a result you'll get a json file with the structure described bellow.

There are two ways to provide data for prediction and three ways you can extract features from your images:

1. **From path**: OML can go recursively over folder and search for any file that has extension ``.jpg``, ``.jpeg``, ``.png``, ``.JPG``, ``.JPEG``, ``.PNG``. It will extract features from the **whole image**;
2. **From dataframe**: You can use csv file with fixed structure in order to extract features from images. In this file you can either add
   **bounding boxes** from which predictions will be made **or** you can remove bounding boxes columns and then prediction will
   be done on the **whole image**.

.. literalinclude:: ../../../examples/inference_images_df.yaml
   :linenos:
   :emphasize-lines: 8-11
   :language: yaml
   :caption: Config for inference on dataframe

**Dataframe structure**:

Inference dataframe follows the same :ref:`format of columns as training/validation<usage-with-custom-dataset>`. But it
is possible to omit some columns except for the mandatory ones.

*Required columns*

- ``path`` - path to image
- Bounding boxes ``x_1``, ``x_2``, ``y_1``, ``y_2`` - integers, the format is ``left``, ``right``, ``top``, ``bot`` (``y_1`` must be less than ``y_2``).
  If only part of your images has bounding boxes, just fill the corresponding row with empty values.
  If all of your images don't have bounding boxes, you can remove or rename these columns in the file.


.. literalinclude:: ../../../examples/inference_images_path.yaml
   :linenos:
   :emphasize-lines: 8-10
   :language: yaml
   :caption: Config for inference on directory with images

.. note::
    Your target config can contain either path to directory with images or path to dataframe. If you'd provide both
   you'll get an error.

    .. literalinclude:: ../../../examples/inference_images_path.yaml
       :diff: ../../../examples/inference_images_df.yaml
       :caption: Either red or green lines allowed at the same time

You also need an inference script in order to make predictions. We suggest you use the following for inference.

.. note::
   Please note that you should create ``config`` directory next to the script and put your config file there. Its name
   should be set in the script on line TODO: set line

.. literalinclude:: ../../../examples/inference.py
   :linenos:
   :language: python
   :caption:

After everything is set up, you can run this script the following way:

.. code-block:: bash
   $ python3 inference.py

As a result you'll get ``json`` file with the following structure:

{
"images_folder": <from config>,
"dataframe_name": <from config>,
"model": <nested dict from config section>,
"transforms": <nested dict from config section>,
"filenames": [file1, fil2, ...],
"features": [vector1, vector2, ...]

