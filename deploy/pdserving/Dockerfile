FROM paddlepaddle/paddle:2.4.2-gpu-cuda11.7-cudnn8.4-trt8.4

WORKDIR /opt

RUN pip install paddle_serving_server_gpu paddle_serving_client paddle_serving_app -i https://mirrors.aliyun.com/pypi/simple/ --no-cache

RUN wget https://paddleocr.bj.bcebos.com/PP-OCRv4/chinese/ch_PP-OCRv4_det_infer.tar -O ch_PP-OCRv4_det_infer.tar; tar -xf ch_PP-OCRv4_det_infer.tar; \
    wget https://paddleocr.bj.bcebos.com/PP-OCRv4/chinese/ch_PP-OCRv4_rec_infer.tar -O ch_PP-OCRv4_rec_infer.tar; tar -xf ch_PP-OCRv4_rec_infer.tar; \
    rm ch_PP-OCRv4_det_infer.tar ch_PP-OCRv4_rec_infer.tar

RUN python3 -m paddle_serving_client.convert --dirname ./ch_PP-OCRv4_det_infer/ \
                                         --model_filename inference.pdmodel          \
                                         --params_filename inference.pdiparams       \
                                         --serving_server ./ppocr_det_v4_serving/ \
                                         --serving_client ./ppocr_det_v4_client/; \
    python3 -m paddle_serving_client.convert --dirname ./ch_PP-OCRv4_rec_infer/ \
                                         --model_filename inference.pdmodel          \
                                         --params_filename inference.pdiparams       \
                                         --serving_server ./ppocr_rec_v4_serving/  \
                                         --serving_client ./ppocr_rec_v4_client/

COPY . .

EXPOSE 9998

ENTRYPOINT [ "python3", "web_service.py" ]
