@Service
public class ValidatorService {

	//uses spring localvalidatorfactory bean so that it can import spring properties
	@Autowired
	private Validator validator;

	private static final Logger logger = LogManager.getLogger(ValidatorService.class.getName());

	public List<String> validate(Object entry) {
		List<String> respMessage = new ArrayList<String>();
		try {
			Errors errors = new BeanPropertyBindingResult(entry, entry.getClass().getName());
			validator.validate(entry, errors);

			if (errors == null || errors.getAllErrors().isEmpty())
				return respMessage;
			else {
				for (ObjectError oe : errors.getAllErrors()) {

					respMessage.add(oe.getDefaultMessage().toString());
				}
			}

		} catch (Exception e) {
			logger.error("ValidatorService exception for :" + e.getMessage());
		}
		return respMessage;
	}

}
